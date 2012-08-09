What are Lenses?
----------------

Lenses are composable functional references.

Ignoring the implementation for the moment, lenses provide us with two operations:

```haskell
view :: Simple Lens a b -> a -> b
set :: Simple Lens a b -> b -> a -> a
```

So we can view a lens as a pair of a getter and a setter that are in some sense compatible.

We'll use the following lenses to start off:

```haskell
_1 :: Simple Lens (a,b) a
_2 :: Simple Lens (a,b) b
```

to both read from

```haskell
>>> view _2 ("hello","world")
("world")
```

and write to parts of a whole:

```haskell
>>> set _2 42 ("hello",0)
("hello",42)
```

Moreover, we can compose lenses with `(.)`.

```haskell
(.) :: Simple Lens a b -> Simple Lens b c -> Simple Lens a c
```

Notice `(.)` composes in the opposite order from what you would expect as a functional programmer, but to an imperative programmer they provide the nice idiom that

```haskell
>>> view (_2._1) ("hello",("world","!!!"))
"world"
```

Finally, you can use `id` as the identity lens

```haskell
id :: Simple Lens a a
```

which just gives you back the value when used with `(^.)` and which when set completely replaces the old value.

They satisfy 3 common-sense laws:

First, that if you put something, you can get it back out

```haskell
view l (set l b a) = b
```

Second that getting and then setting doesn't change the answer

```haskell
set l (view l a) a = a
```

And third, putting twice is the same as putting once, or rather, that the second put wins.

```haskell
set l b1 (set l b2 a) = set l b1 a
```

Note, that the type system isn't sufficient to check these laws for you, so you need to ensure them yourself no matter what lens implementation you use. (Some others will compose with `(.)` the other way.)

We define infix operators to make working with lenses feel more imperative:

```haskell
(^.) :: a -> Simple Lens a b -> b
(.~) :: Simple Lens a b -> b -> a -> a
```

With these you can now use lenses like field accessors.

```literate-haskell
> ("hello",("world","!!!"))^._2._1
"world"
```

You can also write to then in something approaching an imperative style:

```literate-haskell
> _2 .~ 42 $ ("hello",0)
("hello",42)
```

There are also combinators for manipulating parts of the current state for a `State` monad, such as:

```haskell
(.=) :: MonadState a m => SimpleLens a b -> b -> m ()
use  :: MonadState a m => SimpleLens a b -> m b
```

Using these (and other combinators for manipulating state) yields code like the following snippet from the `pong` example included in the distribution (where `p` is in the surrounding scope):

```haskell
check paddle other
    | y >= p^.paddle - paddleHeight/2 && y <= p^.paddle + paddleHeight/2 = do
        ballSpeed._x   %= negate
        ballSpeed._y   += 3*(y - p^.paddle) -- add english
        ballSpeed.both *= speedIncrease
    | otherwise = do
      score.other += 1
      reset
```

More information about how the types for lenses can be derived is available under [Derivation](Derivation).

For now, you may want to explore some code that uses lenses heavily, such as the [Pong Example](https://github.com/ekmett/lens/blob/master/examples/Pong.hs).