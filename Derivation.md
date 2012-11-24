In order to derive the type signatures used in this package and to motivate generalizing these combinators to work on a much wider menagerie of types, we'll take a bit of an excursion through other types and typeclasses you may already be familiar with.

From here, we'll assume that you are acquainted with the types for `Functor`, `Foldable` and `Traversable` and can at least hand-wave the laws for them (see [Typeclassopedia](http://www.haskell.org/haskellwiki/Typeclassopedia)).

The power of (.)
----------------

There is a common folklore pattern for composing `(.)` in Haskell, to compose with a function that takes more and more arguments.

```haskell
(.)         :: (a -> b) -> (c -> a)           -> c           -> b
(.).(.)     :: (a -> b) -> (c -> d -> a)      -> c -> d      -> b
(.).(.).(.) :: (a -> b) -> (c -> d -> e -> a) -> c -> d -> e -> b
```

It generalizes to `fmap` from `Functor`:

```haskell
fmap           :: Functor f
               => (a -> b) -> f a         -> f b
fmap.fmap      :: (Functor f, Functor g)
               => (a -> b) -> f (g a)     -> f (g b)
fmap.fmap.fmap :: (Functor f, Functor g, Functor h)
               => (a -> b) -> f (g (h a)) -> f (g (h b))
```

It also generalizes to `foldMap` from `Foldable`:

```haskell
foldMap                 :: (Foldable f, Monoid m)
                        => (a -> m) -> f a         -> m
foldMap.foldMap         :: (Foldable f, Foldable g, Monoid m)
                        => (a -> m) -> f (g a)     -> m
foldMap.foldMap.foldMap :: (Foldable f, Foldable g, Foldable h, Monoid m)
                        => (a -> m) -> f (g (h a)) -> m
```

And finally (for now) it generalizes to `traverse` from `Traversable`:

```haskell
traverse                   :: (Traversable f, Applicative m)
                           => (a -> m b) -> f a         -> m (f b)
traverse.traverse          :: (Traversable f, Traversable g, Applicative m)
                           => (a -> m b) -> f (g a)     -> m (f (g b))
traverse.traverse.traverse :: (Traversable f, Traversable g, Traversable h, Applicative m)
                           => (a -> m b) -> f (g (h a)) -> m (f (g (h b)))
```

These functions are similar, but slightly different in signature, and provide very different capabilities.

We gain power as we upgrade from `Functor` or `Foldable` to `Traversable`, as evidenced by the fact that we can define `fmap` and `foldMap` given `traverse`. This is done for us directly by `Data.Traversable`, as follows:

```haskell
-- | This function may be used as a value for `fmap` in a `Functor`
--   instance, provided that 'traverse' is defined. (Using
--   `fmapDefault` with a `Traversable` instance defined only by
--   'sequenceA' will result in infinite recursion.)
fmapDefault :: Traversable t => (a -> b) -> t a -> t b
fmapDefault f = runIdentity . traverse (Identity . f)
```

```haskell
-- | This function may be used as a value for `Data.Foldable.foldMap`
-- in a `Foldable` instance.
foldMapDefault :: (Traversable t, Monoid m) => (a -> m) -> t a -> m
foldMapDefault f = getConst . traverse (Const . f)
```

If we rip `traverse` out of their definitions and pass it in as an argument, we can find out exactly what 
properties are needed of `traverse` to make those definitions typecheck. We'll deal with `fmapDefault` first.

Setters
-------

```
ghci> let mapOf t f = runIdentity . t (Identity . f)
ghci> :t mapOf
mapOf :: ((c -> Identity d) -> a -> Identity b) -> (c -> d) -> a -> b
```

So lets define a type alias for that:

```haskell
type Setter a b c d = (c -> Identity d) -> a -> Identity b
```

which lets us write this as:

```haskell
mapOf :: Setter a b c d -> (c -> d) -> a -> b
-- or
mapOf :: ((c -> Identity d) -> a -> Identity b)
      ->  (c -> d)          -> a -> b
mapOf t f = runIdentity . t (Identity . f)
```

(In the actual implementation we rename `Identity` to `Mutator` to provide nicer error messages)

**Setter Laws**

We can write an inverse of `mapOf` fairly mechanically:

```haskell
sets :: ((c -> d) -> a -> b) -> Setter a b c d
-- or
sets :: ((c -> d)          -> a -> b)
     ->  (c -> Identity d) -> a -> Identity b
sets l f = Identity . l (runIdentity . f)
```

It is trivial to verify that

```haskell
sets . mapOf = id
mapOf . sets = id
```

but if we want the `Setter` to act like a `Functor`, the argument the user supplies to sets
should satisfy a version of the `Functor` laws. In particular, we'd like:

```haskell
mapOf l id = id
mapOf l f . mapOf l g = mapOf l (f . g)
```

so the function `m` you supply to sets must satisfy

```haskell
m id = id
m f . m g = m (f . g)
```

(Note: Unlike with `fmap` we have to actually check both laws, because parametricity doesn't help
us derive the second for free from the first.)

So, what satisfies the functor laws out of the box? `fmap`!

```haskell
mapped :: Functor f => Setter (f a) (f b) a b
-- or
mapped :: Functor f => (a -> Identity b) -> f a -> Identity (f b)
mapped = sets fmap
```

It is trivial then to check that

```haskell
fmap = mapOf mapped
```

Now we can write a number of combinators that are parameterized on the `Functor`-like construction they work over.

Lets consider the limited form of mapping allowed on the arrays provided by `Data.Array.IArray`:

```haskell
amap :: (IArray a e', IArray a e, Ix i) => (e' -> e) -> a i e' -> a i e
```

Given this, we can derive:

```haskell
amapped :: (IArray a c, IArray a d, Ix i) => Setter (a i c) (a i d) c d
-- or
amapped :: (IArray a c, IArray a d, Ix i)
        => (c -> Identity d) -> a i c -> Identity (a i d)
amapped = sets amap
```

Then we can use

```haskell
mapOf amapped :: (IArray a c, IArray a d, Ix i) => (c -> d) -> a i c -> a i d
```

instead of `amap`.

We can also pass in the type of `map` provided by, say, `Data.Text.map`

```haskell
tmapped :: Setter Text Text Char Char
-- or
tmapped :: (Char -> Identity Char) -> Text -> Identity Text
tmapped = sets Data.Text.map
```

And it follows that:

```haskell
mapOf tmapped :: (Char -> Char) -> Text -> Text
```

We haven't gained much power over just passing in the functions `amap` or `Data.Text.map` directly, yet, but we have gained two things:

* The composition of two setters, such as:

  ```haskell
  mapped.mapped :: (Functor f, Functor g) => (a -> Identity b) -> f (g a) -> Identity (f (g b))
  ```

  is still a valid `Setter`!

  ```haskell
  mapped.mapped :: (Functor f, Functor g) => Setter (f (g a)) (f (g b)) a b
  ```

  So you can use `mapOf (mapped.mapped)` to recover the original `fmap.fmap` above. This lets you get away without using `Compose` to manually bolt functors to meet the shape requirements.

* Another thing that we have won is that if we have a `Traversable` container, we can pass its `traverse` in
to `mapOf` instead of a `Setter` for the container.

Setters form a category, using `(.)` and `id` for composition and identity, but you can use the existing `(.)` and `id` from the `Prelude` for them (though they compose backwards).

However, to gain that power we traded in other functionality. Knowing `f` is a `Functor` lets us instantiate the type arguments `a` and `b` to anything we want, over and over again, we also need to manually check any of the formerly free theorems we want to use with out `Setter`.

Many combinators for these are provided in [`Control.Lens.Setter`](https://github.com/ekmett/lens/blob/master/src/Control/Lens/Setter.hs).

As an aside, we can now define the `(.~)` (and `set`) combinators we used during the introduction:

```haskell
(.~), set :: Setter a b c d -> d -> a -> b
l .~ d = runIdentity . l (Identity . const d)
set = (.~)
```

In a few moments we'll see how they can be applied to a `Lens`, but first:

Folds
-----

Now lets apply the same treatment to the other default definition supplied by `Data.Traversable`:

```haskell
foldMapDefault :: (Traversable t, Monoid m) => (a -> m) -> t a -> m
foldMapDefault f = getConst . traverse (Const . f)
```

If we plug in an argument for `traverse` and rip off the type signature, we get

```
ghci> let foldMapOf t f = getConst . t (Const . f)
ghci> :t foldMapOf
foldMapOf :: ((c -> Const m d) -> a -> Const m b) -> (c -> m) -> f c -> m
```

The second argument to `Const` is polymorphic in each case, but to avoid dangling type variables we'll eliminate them by making them match.

Writing it out, and making up a type alias:

```haskell
type Getting m a c = (c -> Const m c) -> a -> Const m a
```

(Note: In the actual implementation `Const` is renamed to `Accessor` for error reporting reasons)

we can make the slightly nicer looking type

```haskell
foldMapOf :: Getting m a c -> (c -> m) -> a -> m
-- or
foldMapOf :: ((c -> Const m c) -> a -> Const m a)
          ->  (c -> m)         -> a -> m
foldMapOf l f = getConst . l (Const . f)
```

It follows by substitution that

```haskell
foldMapDefault = foldMapOf traverse
```

we could define an inverse of `foldMapOf` as we did with `mapOf`, above, etc.

```haskell
folds :: ((c -> m) -> a -> m) -> Getting m a c
-- or
folds :: ((c -> m)         -> a -> m)
      ->  (c -> Const m c) -> a -> Const m a
folds l f = Const . l (getConst . f)
```

If follows very easily that

```haskell
folds . foldMapOf = id
foldMapOf . folds = id
```

and so we can see that we can pass something other than `traverse` to `foldMapOf`:

```haskell
type Fold a b c d = forall m. Monoid m => (c -> Const m d) -> a -> Const m b
```

```haskell
folded :: Foldable f => Fold (f c) b c d
-- or
folded :: (Foldable f, Monoid m) => (c -> Const m d) -> f c -> Const m b
folded = folds foldMap
```

And we can mechanically verify that `foldMap = foldMapOf folded` by:

```haskell
foldMap = (foldMapOf . folds) foldMap = foldMapOf (folds foldMap) = foldMapOf folded
```

There are no laws for `Foldable` that do not follow directly from the types, and the same
holds for a `Fold`.

We can define all of the combinators in `Data.Foldable` in terms of `foldMap`, so we in turn
define them in terms of an arbitrary `Fold` in [`Control.Lens.Fold`](https://github.com/ekmett/lens/blob/master/src/Control/Lens/Fold.hs).

(In the actual implementation the type of `Fold` is changed to use a typeclass constraint rather
than an explicit `Const m` to yield nicer error messages when you attempt to use a `Setter` as
a `Fold` and to permit the use of certain `Applicative` transformers as `Monoid` transformers.)

As with `Setter`, the composition of two folds using `(.)` is a valid `Fold`, and `id` is the identity fold that returns the container itself as its only result.

Traversals
----------

Given the signatures of `Fold` and `Setter`, we can derive something that can be used as both -- after all
`traverse` from `Traversal` served this function originally!

Given the `Monoid` m, `Const m` forms an `Applicative`, and `Identity` is also `Applicative`.

So substituting back in to the definitions above, we find:

```haskell
type SimpleTraversal a c = forall f. Applicative f => (c -> f c) -> a -> f a
```

But this is weaker than what we started with, since

```haskell
traverse :: (Traversable t, Applicative f) => (c -> f d) -> t c -> f (t d)
```

Picking

```haskell
type Traversal a b c d = forall f. Applicative f => (c -> f d) -> a -> f b
```

we get something that subsumes

```haskell
traverse :: Traversable t => Traversal (t a) (t b) a b
-- or
traverse :: (Traversable t, Applicative f) => (a -> f b) -> t a -> f (t b)
```

We'll still be dealing with those 2-argument traversals a lot, along with their `Setter` equivalents, so we'll define

    type Simple f a b = f a a b b

(Note: 'Simple' 'Traversal' partially applies a type synonym. This is only legal with the `LiberalTypeSynonyms` extension turned on, but
GHC allows it so long as the type synonym contains a higher-rank type, even without `LiberalTypeSynonyms`.)

But, what happened?

When I picked the type for `Getting`, I only used two type arguments.

```haskell
type Getting m a c = (c -> Const m c) -> a -> Const m a
```

This hints that for something to be a valid `Traversal` it should be possible to choose `a ~ b`, and `c ~ d` and get a meaningful traversal. In fact the `Traversable` laws, which we still want to have hold for a `Traversal`, tell us that:

```haskell
t pure = pure
Compose . fmap (t f) . t g = t (Compose . fmap f . g)
```


And the first of those laws requires `a ~ b`, `c ~ d` to be a possible choice of the type arguments to your `Traversal`. We implement this polymorphic overloading of traversals in a fairly ad hoc way, by just making the user provide the "family structure" for us.

Given that our `Traversal` satisfies the `Traversable` laws, the laws for `Setter` immediately follow, and `Fold` had no extra laws to check.

So what else can we build a `Traversal` for?

We can traverse both elements in a tuple:

```haskell
both :: Traversal (a,a) (b,b) a b
-- or
both :: Applicative f => (a -> f b) -> (a,a) -> f (b,b)
both f (a,b) = (,) <$> f a <*> f b
```

The left side of an `Either`:

```haskell
traverseLeft :: Traversal (Either a c) (Either b c) a b
-- or
traverseLeft :: Applicative f => (a -> f b) -> Either a c -> f (Either b c)
traverseLeft f (Left a) = Left <$> f a
traverseLeft f (Right b) = pure $ Right b
```

And we can use a `Traversal` to update element (if it exists) at a given position in a `Map`, etc.

We can compose two traversals with each other using `(.)` as we did at the very start, but now the composition forms a valid `Traversal`.

```haskell
traverse.traverse :: (Traversable f, Traversable g) => Traversal (f (g a)) (f (g b)) a b
```

but we can also compose them with a `Setter` or `Fold`, yielding a `Setter` or `Fold` in turn.

Unlike with mapped and folded, we will often want to use the `Traversal` directly as a combinator. 

Moreover, all the intuition you have for how to write `Traversable` instances transfers immediately to how to write a `Traversal`.

There are a number of combinators for working with traversals in [`Control.Lens.Traversal`](https://github.com/ekmett/lens/blob/master/src/Control/Lens/Traversal.hs)

We're almost ready for lenses, but first we have one more diversion.

Getters
-------

If we convert a function from `(a -> c)` to continuation passing style, we get

```haskell
cps :: (a -> c) -> (c -> r) -> a -> r
cps f g = g . f
```

That is to say, rather than return a result `c`, it takes a function from `(c -> m)` and calls it with the answer instead.

If we have a CPS'd function that is polymorphic in its return type, we can get the original function back out:

```haskell
uncps :: (forall r. (c -> r) -> a -> r) -> a -> c
uncps f = f id
```

Finally, we can prove a number of properties:

```haskell
uncps . cps = id
cps . uncps = id
cps id = id
cps f . cps g = cps (g . f)
```

Now, we can relax the type of uncps slightly to

```haskell
uncps' :: ((c -> c) -> a -> c) -> a -> c
uncps' f = f id
```

but now we no longer know that our function `f :: (c -> c) -> a -> c` can't be doing something
to combine the results of the function you pass it.  We lose the `cps . uncps = id` law and only have:

```haskell
uncps' . cps  = id
```

Now, let's compose them like we did for `fmap`, `traverse` and `foldMap` at the start:

If we start with 3 functions:

```haskell
f :: A -> B
g :: B -> C
h :: C -> D
```

Then

```haskell
cps f :: (B -> r) -> (A -> r)
cps g :: (C -> r) -> (B -> r)
cps h :: (D -> r) -> (C -> r)
```

And when we compose these functions between functions, we obtain:

```haskell
cps f                 :: (B -> r) -> (A -> r)
cps f . cps g         :: (C -> r) -> (A -> r)
cps f . cps g . cps h :: (D -> r) -> (A -> r)
```

Earlier we provided a type for consuming a `Fold`:

```haskell
type Getting r a c = (c -> Const r c) -> a -> Const r a
```

What we want (so that uncps can work) is something completely polymorphic in `r`.

```haskell
type Getter a c = forall r. (c -> Const r c) -> a -> Const r a
```

Along the way, we get an interesting result, A `Getter` is just a `Fold` that doesn't use the `Monoid`! Recall:

```haskell
type Fold a c = forall r. Monoid r => (c -> Const r c) -> a -> Const r a
```

We can go back and define `(^.)` now, and empower it to consume either a `Fold` or `Getter` or `Traversal`.

```haskell
(^.) :: a -> Getting c a c -> c
a ^. l = getConst (l Const a)
```

Remember, we can consume a `Traversal` because every `Traversal` is a valid `Fold`, just like every `Getter` is a valid `Fold`.

Also note that `(^.)` doesn't require anything of `c`!

When it gets applied the argument l will demand the properties of `c` that it needs:

For instance when we apply `(^.)` to a `Fold`, it will demand a `Monoid` instance for `c`:

```haskell
(^.folded) :: (Foldable f, Monoid m) => f m -> m
```

Also, since, a `Monoid m` is needed to satisfy the `Applicative` for `Const m`,

```haskell
(^.traverse) :: (Traversable t, Monoid m) => t m -> m
```

But we can use `(^.)` to access a `Getter`, without any restrictions!

There are a number of combinators for working with getters in [`Control.Lens.Getter`](https://github.com/ekmett/lens/blob/master/src/Control/Lens/Getter.hs).

We can use a `Getter` as a `Fold`, but it is not a valid `Traversal` or `Setter`, nor is a `Traversal` or `Setter` a `Getter`.

With all of that we're finally ready to define

Lenses
------

A `Lens` is a `Traversal` that can also be used as a `Getter`. This means it can be used as a `Setter` and a `Fold` as well.

Recall that a Getter was a `Fold` that can't use the `Monoid`.

Without the `Monoid`, all that `Const` and `Identity` have in common is that each is a `Functor`.


```haskell
type Lens a b c d = forall f. Functor f => (c -> f d) -> a -> f b
```

We inherit the `Traversal` laws, so we know for a `Lens` `l`

```haskell
l pure = pure
Compose . fmap (l f) . l g = l (Compose . fmap f . g)
```

and we also know that a `Lens a b c d` can be used as a function from `(a -> c)`.
