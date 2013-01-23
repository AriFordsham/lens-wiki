We keep finding bugs in GHC as we develop `lens`.

I've started a rogues' gallery here:

* [RULES don't apply to a newtype constructor](http://hackage.haskell.org/trac/ghc/ticket/7398)
* [Derived Foldable and Traversable instances become extremely inefficient due to eta-expansion ](http://hackage.haskell.org/trac/ghc/ticket/7436)
* [A strictness bug in `ByteString`](http://hackage.haskell.org/trac/ghc/ticket/7556)
* [GHC doesn't optimize (strict) composition with `id`](http://hackage.haskell.org/trac/ghc/ticket/7542)
* [`unsafeCoerce` without extensions](http://hackage.haskell.org/trac/ghc/ticket/7453)
* [Exciting arity](http://hackage.haskell.org/trac/ghc/ticket/5929) warnings can be triggered by the heavy use of `lens` in GHC 7.4.x and below.