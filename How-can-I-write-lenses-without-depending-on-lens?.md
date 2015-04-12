Part of the magic of `lens` (referring to the package) is that you can
write lenses (referring to the concept) without actually depending on
`lens`.

This page provides a guide for package maintainers who'd like to add
`lens`-compatible lenses to their library incurring minimal
dependencies. It covers all of the basic lens types: Lens, Prism, Iso,
Traversal, Fold, Getter, ~~Setter~~, ~~Review~~, ~~Action~~, and
~~Monadic Fold~~.

The various guides follow, ordered by needed dependencies.

**A note on inlining**. Generally, all lens-like functions get their
best performance when aggressively `INLINE`'d. GHC can infer inlining
within a module, but the `INLINE` pragma is recommended for all
lens-like functions so that they'll be inlined in other modules as
well. (`<edwardk> INLINE all the things // lens really really needs to inline`)

Lens
====

*No dependencies.*

The simplest interface is Lens. This is useful for exposing record
fields or pieces of internal structure of your data types. The Lens
type is

```haskell
type Lens s t a b = Functor f => (a -> f b) -> s -> f t
```

which is very similar to `Data.Traversable.traverse` but can be built
for traversals which do not necessarily traverse over slots denoted in
the type signature. You write Lenses in much the same way you write
`Traversable` instances, but using only the `Functor` interface

```haskell
data State = State !Word8 !Word8

-- _word1 :: Lens' State Word8
_word1 :: Functor f => (Word8 -> f Word8) -> State -> f State
_word1 inj (State wa wb) = flip State wb <$> inj wa
{-# INLINE _word1 #-}
```

Generally, you must apply the `inj :: a -> f b` function to the
subcomponent of your type you wish to focus on with your Lens then use
`fmap` to reconstruct your type inside of the `Functor`.


Traversal
=========

*No dependencies.*

If you'd like to provide sequential access to several like-typed
subcomponents of your type, build a `Traversal`. The technique is
identical to `Lens` except generalized to use the greater power
afforded by `Applicative`. The `Traversal` type is

```haskell
type Traversal s t a b = Applicative f => (a -> f b) -> s -> f t
```

So, we now know that the `inj` function gives us an `Applicative` and
thus we can use it repeatedly before reassembling our type.

```haskell
data State = State !Word8 !Word8

-- traverseWords :: Traverasl' State Word8
traverseWords :: Applicative f => (Word8 -> f Word8) -> State -> f State
traverseWords inj (State wa wb) = State <$> inj wa <*> inj wb
{-# INLINE traverseWords #-}
```

Like Lens, this is similar to writing a `Traversable` instance
(unsurprisingly) but even closer since we can use `(<*>)`.


Getter
======

*(Technically) depends on `contravariant` which includes `tagged`,
 `transformers`, `transformers-compat`, `mtl`, but...*

Getters are not usually useful to define as a package
maintainer. Users of lens can build getters on the fly by using `to ::
(s -> a) -> IndexPreservingGetter s a` if you provide a function from
the larger type to the subcomponent.


Fold
====

*Depends on `contravariant` which includes `tagged`, `transformers`,
 `transformers-compat`, `mtl`.*
 
Folds are a summary of a Traversal. They arise when after applying
`inj` to your summary value it's no longer possible to reconstruct
your original type. Yet, they still have a type which ends up
returning a `Functor` "containing" your reconstructed type.
 
```haskell
type Fold s a = 
    (Contravariant f, Applicative f) => (a -> f a) -> s -> f s
```

To write a Fold you use a trick afforded by the `Contravariant` and
`Applicative` instances together---they force the parameter of `f` to
be phantom by letting you write a strange internal function

```haskell
import Data.Functor.Contravariant (contramap)

coerce :: (Contravariant f, Applicative f) => f a -> f b
coerce = contramap (const ()) . fmap (const ())
{-# INLINE coerce #-}
```

Then we use this to ignore our usual obligation to rebuild the total
structure.

```haskell
data State = State !Word8 !Word8

-- mixing :: Fold State Word8
mixing :: 
  (Contravariant f, Applicative f) => (Word8 -> f Word8) -> State -> f State
mixing inj (State wa wb) = coerce . inj $ wa `xor` wb
{-# INLINE mixing #-}
```


Iso
===

*Depends on `profunctors` which includes `tagged`, `comonad`,
 `transformers`, `containers`, `deepseq`, `array`, and `nats`.*

An Iso is an isomorphism between two types. It's useful whenever you
have two representations of the same type, each perhaps exposing a
different access pattern or aspect, but you have two functions `to ::
A -> B` and `fro :: B -> A` which are mutual inverses.

```haskell
type Iso s t a b =
  (Profunctor p, Functor f) => p a (f b) -> p s (f t)
```

Isos are easy to construct using only your two functions `to` and
`fro` and 

```haskell
dimap :: Profunctor p => (s -> a) -> (b -> t) -> p a b -> p s t
```

For instance, the running example `State` is isomorphic to a pair of
`Word8`s.

```haskell
data State = State !Word8 !Word8

-- asAPair :: Iso' State (Word8, Word8)
asAPair :: (Functor f, Profunctor p) =>
           p (Word8, Word8) (f (Word8, Word8)) -> p State (f State)
asAPair = dimap (\(State wa wb) -> (wa, wb)) (fmap $ uncurry State)
{-# INLINE asAPair #-}
```

Note the use of `fmap` on the second function necessary for lifting
your isomorphism witness up into the `Functor`.


Prism
=====

*Depends on `profunctors` which includes `tagged`, `comonad`,
 `transformers`, `containers`, `deepseq`, `array`, and `nats`.*

Prisms usually target one branch of a sum type. In order to qualify,
they must be able to "maybe" access the branch that they're focused on
*and* be able to review their target type to reconstruct the whole.

They have the type

```haskell
type Prism s t a b = (Choice p, Applicative f) => p a (f b) -> p s (f t)
```

and we'll construct a new example using a sum type. Prisms are built
like Isos, but since our isomorphisms is partial in one direction, the
`Choice` constraint is used to give that flexibility.

```haskell
data Doing a = Done a | Doing (Doing a)

-- using right' from package profunctors
-- 
--   class Profunctor p => Choice p where
--     right' :: p a b -> p (Either c a) (Either c b)
--     ...

-- _Done :: Prism' (Doing a) a
_Done :: (Choice p, Applicative f) => p a (f a) -> p (Doing a) (f (Doing a))
_Done = dimap to fro . right' where
  to (Done a)    = Right a
  to x           = Left x
  fro (Left it)  = pure it
  fro (Right fa) = Done <$> fa
{-# INLINE _Done #-}
```

Here, the only degrees of freedom are in the `Right`y branches of `to`
and `from`. In fact, this pattern is completely captured by the `lens`
function `prism` usually used to construct Prisms. It can be easily
included as a helper function in your library if you're defining many
Prisms.

```haskell
prism :: 
  (Choice p, Applicative f) => 
  (b -> t) -> (s -> Either t a) -> p a (f b) -> p s (f t)
prism bt seta = dimap seta (either pure (fmap bt)) . right'
{-# INLINE prism #-}
```

---

*Todo:*

1. generalize this to the 4-parameter `s t a b` formulations.

2. finish out the other types