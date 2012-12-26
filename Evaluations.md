# Manual Evaluations

This page contains manual evaluations of various interesting function calls to
demonstrate what is going on in the internals of Lens.

## Iso

### `view (iso (*10) (/10)) 5`

```haskell
view (iso (*10) (/10)) 5

getConst $ iso (*10) (/10) Const 5

getConst $ (lmap (*10) . rmap (fmap (/10))) Const 5

getConst $ lmap (*10) (rmap (fmap (/10)) Const) 5

getConst $ flip (.) (*10) (fmap (/10) . Const) 5

getConst $ (fmap (/10) . Const . (*10)) 5

(*10) 5
```

### `over (iso (*10) (/10)) (+1) 5`

```haskell
over (iso (*10) (/10)) (+1) 5

runIdentity $ iso (*10) (/10) (Identity . (+1)) 5

runIdentity $ (lmap (*10) . rmap (fmap (/10))) (Identity . (+1)) 5

runIdentity $ lmap (*10) (rmap (fmap (/10)) (Identity . (+1))) 5

runIdentity $ flip (.) (*10) (fmap (/10) . Identity . (+1)) 5

runIdentity $ (fmap (/10) . Identity . (+1) . (*10)) 5

(/10) . (+1) . (*10) $ 5
```

### `from (iso (*10) (/10))`

```haskell
from (iso (*10) (/10))

case runIso (iso (*10) (/10)) of
  (sa, bt) -> iso bt sa

case runExchange (iso (*10) (/10) (Exchange (id, Identity))) of
  (sa, bt) -> iso (runIdentity . bt) sa

case runExchange ((lmap (*10) . rmap (fmap (/10))) (Exchange (id, Identity))) of
  (sa, bt) -> iso (runIdentity . bt) sa

case runExchange (lmap (*10) (rmap (fmap (/10)) (Exchange (id, Identity)))) of
  (sa, bt) -> iso (runIdentity . bt) sa

case runExchange (lmap (*10) (Exchange (id, fmap (/10) . Identity))) of
  (sa, bt) -> iso (runIdentity . bt) sa

case runExchange (Exchange (id . (*10), fmap (/10) . Identity)) of
  (sa, bt) -> iso (runIdentity . bt) sa

case (id . (*10), fmap (/10) . Identity) of
  (sa, bt) -> iso (runIdentity . bt) sa

iso (runIdentity . fmap (/10) . Identity) (id . (*10))

iso (/10) (*10)
```
