**The power of (.)**

There is a common folklore pattern for composing `(.)` in Haskell, to compose with a function that takes more and more arguments.

    (.)         :: (a -> b) -> (c -> a)           -> c           -> b
    (.).(.)     :: (a -> b) -> (c -> d -> a)      -> c -> d      -> b
    (.).(.).(.) :: (a -> b) -> (c -> d -> e -> b) -> c -> d -> e -> b

It generalizes to 'fmap'.

    fmap           :: Functor f
                   => (a -> b) -> f a         -> f b
    fmap.fmap      :: (Functor f, Functor g)
                   => (a -> b) -> f (g a)     -> f (g b)
    fmap.fmap.fmap :: (Functor f, Functor g, Functor h)
                   => (a -> b) -> f (g (h a)) -> f (g (h a))

It also generalizes to 'foldMap':

    foldMap                 :: (Foldable f, Monoid m)
                            => (a -> m) -> f a         -> m
    foldMap.foldMap         :: (Foldable f, Foldable g, Monoid m)
                            => (a -> m) -> f (g a)     -> m
    foldMap.foldMap.foldMap :: (Foldable f, Foldable g, Foldable h, Monoid m)
                            => (a -> m) -> f (g (h a)) -> m

And it generalizes to 'traverse':

    traverse                   :: (Traversable f, Applicative m)
                               => (a -> m b) -> f a         -> m (f b)
    traverse.traverse          :: (Traversable f, Traversable g, Applicative m)
                               => (a -> m b) -> f (g a)     -> m (f (g b))
    traverse.traverse.traverse :: (Traversable f, Traversable g, Traversable h, Applicative m)
                               => (a -> m b) -> f (g (h a)) -> m (f (g (h b)))

These functions are similar, but slightly different in signature, and provide very different capabilities.

We gain 'power' as we upgrade from Functor or Foldable to Traversable, as evidenced by the fact that we can define 'fmap' and 'foldMap' given 'traverse'.

These definitions are provided for us directly by Data.Traversable, as follows:

    -- | This function may be used as a value for `fmap` in a `Functor`
    --   instance, provided that 'traverse' is defined. (Using
    --   `fmapDefault` with a `Traversable` instance defined only by
    --   'sequenceA' will result in infinite recursion.)
    fmapDefault :: Traversable t => (a -> b) -> t a -> t b
    fmapDefault f = runIdentity . traverse (Identity . f)

    -- | This function may be used as a value for `Data.Foldable.foldMap`
    -- in a `Foldable` instance.
    foldMapDefault :: (Traversable t, Monoid m) => (a -> m) -> t a -> m
    foldMapDefault f = getConst . traverse (Const . f)

If we rip 'traverse' out of their definitions and pass it in, we can find out exactly what 
properties are needed of 'traverse' to make these typecheck:

    ghci> let mapOf t f = runIdentity . t (Identity . f)
    ghci> :t mapOf
    mapOf :: ((c -> Identity d) -> a -> Identity b) -> (c -> d) -> a -> b

So lets define a type alias for that:

    type Setter a b c d = (c -> Identity d) -> a -> Identity b

which lets us write this as:

    mapOf :: Setter a b c d -> (c -> d) -> a -> b
    mapOf t f = runIdentity . t (Identity . f)

(In the actual implementation we rename Identity to 'Mutator' to provide nicer error messages)

***Setter Laws***

We can write an inverse of mapOf fairly mechanically:

    sets :: ((c -> d) -> a -> b) -> Setter a b c d
    sets l f a = Identity . l (runIdentity . f)

It is trivial to verify that

    sets . fmapOf = id
    fmapOf . sets = id

but if we want the Setter to act like a Functor, the argument the user supplies to sets
should satisfy a version of the Functor laws. In particular, we'd like:

    mapOf l id = id
    mapOf l f . mapOf l g = mapOf l (f . g)

so the function @m@ you supply to sets must satisfy

    m id = id
    m f . m g = m (f . g)

(Note: Unlike with fmap we have to actually check both laws, because parametricity doesn't help
us derive the second for free from the first.)

So, what satisfies the functor laws out of the box? fmap!

    mapped :: Functor f => Setter (f a) (f b) a b
    mapped = sets fmap

If is trivial then to check that

    fmap = mapOf mapped

Now we can write a number of combinators that are parameterized on the 'Functor'-like construction they work over.

Lets consider the limited form of mapping allowed on the arrays provided by `Data.Array.IArray`:

    amap :: (IArray a e', IArray a e, Ix i) => (e' -> e) -> a i e' -> a i e

Given this, we can derive:

    amapped :: (IArray a c, IArray a d) => Setter (a i c) (a i d) c d
    amapped = sets amap

Then we can use

    mapOf amapped :: (IArray a c, IArray a d) => (c -> d) -> a i c -> a i d

instead of `amap`.

We can also pass in the type of 'map' provided by Data.ByteString.map

    mapOf Data.ByteString.map :: (Char -> Char) -> ByteString -> ByteString

We haven't gained much power over just passing in the functions amap or Data.ByteString.map directly, yet.

We have however, gained two things.

* The composition of two Setters, such as:

    mapped.mapped :: (Functor f, Functor g) => (a -> Identity b) -> f (g a) -> Identity (f (g b))

  is still a valid Setter!

    mapped.mapped :: (Functor f, Functor g) => Setter (f (g a)) (f (g b)) a b

  So you can use `mapOf (mapped.mapped)` to recover the original `fmap.fmap` above. This lets you get away without using 'Compose' to manually bolt functors together.

* Another thing that we have won is that if we have a 'Traversable' container, we can pass its 'traverse' in
to 'mapOf' instead of a 'Setter' for the container.

Setters form a category, using (.) and id for composition and identity, but you can use the existing (.) and id from the Prelude for them.

Many combinators for these are provided in Control.Lens.Setter.

**Folds**

Now lets apply the same treatment to the other default definition supplied by Data.Traversable:

    foldMapDefault :: (Traversable f, Monoid m) => (a -> m) -> f a -> f b
    foldMapDefault f = getConst . traverse (Const . f)

If we plug in an argument for 'traverse' and rip off the type signature, we get

    ghci> let foldMapOf t f = getConst . t (Const . f)
    ghci> :t foldMapOf
    foldMapOf :: (c -> Const m d) -> a -> Const m b) -> (c -> m) -> f c -> m

Writing it out, and making up a type alias:

    type Getting m a b c d = (c -> Const m d) -> a -> Const m b

(Note: In the actual implementation Const is renamed to 'Accessor' for error reporting reasons)

we can make the slightly nicer looking type

    foldMapOf :: Getting m a b c d -> (c -> m) -> a -> m
    foldMapOf l f = getConst . l (Const . f)

It follows by substitution that

    foldMapDefault = foldMapOf traverse

we could define an inverse of foldMapOf as we did with fmapOf, above, etc.

    folds :: ((c -> m) -> a -> m) -> Getting m a b c d
    folds l f = Const . l (getConst . f)

If follows very easily that

    folds . foldMapOf = id
    foldMapOf . folds = id

and so we can see that we can pass something other than 'traverse' to 'foldMapOf':

    type Fold a b c d = forall m. Monoid m => (c -> Const m d) -> a -> Const m b

    folded :: Foldable f => Fold (f c) b c d
    folded = folds foldMap

And we can mechanically verify that foldMap = foldMapOf folded

    foldMap = (foldMapOf . folds) foldMap = foldMapOf (folds foldMap) = foldMapOf folded

There are no laws for 'Foldable' that do not follow directly from the types, and the same
holds for a 'Fold'.

We can define all of the combinators in Data.Foldable in terms of foldMap, so we can in turn
define them in terms of an arbitrary 'Fold' in Control.Lens.Fold.

(In the actual implementation the type of Fold is changed to use a typeclass constraint rather
than an explicit 'Const m' to yield nicer error messages when you attempt to use a 'Setter' as
a 'Fold')

**Traversals**

Given the signatures of Fold and Setter, we can derive something that can be used as both -- after all
`traverse` from `Traversal` served this function originally!

Given the `Monoid` m, `Const m` forms an `Applicative`, and 'Identity' is also 'Applicative'.

So substituting back in to the definitions above, we find:

    type Traversal a b c d = forall f. Applicative f => (c -> f d) -> a -> f b

Then

    traverse :: Traversable t => Traversal (t a) (t b) a b

We still want the 'Traversable' laws to hold for a 'Traversal', so we get the side conditions that for a traversal @t@:

    t pure = pure
    Compose . fmap (t f) . t g = t (Compose . fmap f . g)

(There are also some less algebraic properties but commonsense properties that a valid Traversable should satisfy that are described in Jeremy Gibbons' paper, "The Essence of the Iterator Pattern")

From these the laws for 'Setter' immediately follow, and 'Fold' had no extra laws to check.

So what else can we build a 'Traversal' for?

We can traverse both elements in a pair:

    both :: Traversal (a,a) (b,b) a b
    both f (a,a) = (,) <$> f a <*> f b

The left side of an Either:

    traverseLeft :: Traversal (Either a c) (Either b c) a b
    traverseLeft f (Left a) = Left <$> f a
    traverseLeft f (Right b) = pure $ Right b

And we can use a 'Traversal' to update element (if it exists) at a given position in a map, etc.

We can compose these with another Traversal using (.) as we did at the very start, but we can also compose them with Setters or Folds!

Moreover, all the intuition you have for how to write 'Traversable' instances transfers immediately to how to write a 'Traversal'.
