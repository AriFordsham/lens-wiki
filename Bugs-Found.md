We keep finding bugs in GHC as we develop `lens`.

I've started a rogues gallery here:

* A strictness bug in `ByteString`: http://hackage.haskell.org/trac/ghc/ticket/7556
* GHC doesn't optimize (strict) composition with id: http://hackage.haskell.org/trac/ghc/ticket/7542
* unsafe coerce without extensions: http://hackage.haskell.org/trac/ghc/ticket/7453