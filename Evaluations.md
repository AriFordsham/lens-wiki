# Manual Evaluations

This page contains manual evaluations of various interesting function calls to
demonstrate what is going on in the internals of Lens.

Some functions have been simplified by making them less generic than the actual
versions in Lens for pedagogical reasons.

* `Const` ≡ `Accessor`
* `Identity` ≡ `Mutator`

## Lens

### `both . both`

(Based on pre-3.8, 2012-12-26)

```haskell
both . both

-- both f (x,y) = (,) <$> f x <*> f y
both . (\f (x,y) -> (,) <$> f x <*> f y)

\f -> both (\(x,y) -> (,) <$> f x <*> f y)

\f -> (\g (ab,cd) -> (,) <$> g ab <*> g cd) (\(x,y) -> (,) <$> f x <*> f y)

\f -> \(ab,cd) -> (,) <$> (\(a,b) -> (,) <$> f a <*> f b) ab
                      <*> (\(c,d) -> (,) <$> f c <*> f d) cd

\f ((a,b),(c,d)) -> (,) <$> ((,) <$> f a <*> f b)
                        <*> ((,) <$> f c <*> f d)
```

## Iso

### `view (iso (*10) (/10)) 5`

(Based on pre-3.8, 2012-12-26)

```haskell
view (iso (*10) (/10)) 5

-- view l = getConst . l Const
getConst $ iso (*10) (/10) Const 5

-- iso sa bt = lmap sa . rmap (fmap bt)
getConst $ (lmap (*10) . rmap (fmap (/10))) Const 5

getConst $ lmap (*10) (rmap (fmap (/10)) Const) 5

-- lmap{(->)} = flip (.)
-- rmap{(->)} = (.)
getConst $ flip (.) (*10) (fmap (/10) . Const) 5

getConst $ (fmap (/10) . Const . (*10)) 5

(*10) 5
```

### `over (iso (*10) (/10)) (+1) 5`

(Based on pre-3.8, 2012-12-26)

```haskell
over (iso (*10) (/10)) (+1) 5

-- over l f = runIdentity . l (Identity . f)
runIdentity $ iso (*10) (/10) (Identity . (+1)) 5

-- iso sa bt = lmap sa . rmap (fmap bt)
runIdentity $ (lmap (*10) . rmap (fmap (/10))) (Identity . (+1)) 5

runIdentity $ lmap (*10) (rmap (fmap (/10)) (Identity . (+1))) 5

-- lmap{(->)} = flip (.)
-- rmap{(->)} = (.)
runIdentity $ flip (.) (*10) (fmap (/10) . Identity . (+1)) 5

runIdentity $ (fmap (/10) . Identity . (+1) . (*10)) 5

(/10) . (+1) . (*10) $ 5
```

### `from (iso (*10) (/10))`

(Based on pre-3.8, 2012-12-26)

```haskell
from (iso (*10) (/10))

-- from k = case runIso k of (sa, bt) -> iso bt sa
case runIso (iso (*10) (/10)) of
  (sa, bt) -> iso bt sa

-- newtype Exchange a b s t = Exchange { runExchange :: (s -> a, b -> t) }

-- runIso ai = case runExchange (ai (Exchange (id, Identity))) of
--   (sa, bt) -> (sa, runIdentity . bt)
case runExchange (iso (*10) (/10) (Exchange (id, Identity))) of
  (sa, bt) -> iso (runIdentity . bt) sa

-- iso sa bt = lmap sa . rmap (fmap bt)
case runExchange ((lmap (*10) . rmap (fmap (/10))) (Exchange (id, Identity))) of
  (sa, bt) -> iso (runIdentity . bt) sa

case runExchange (lmap (*10) (rmap (fmap (/10)) (Exchange (id, Identity)))) of
  (sa, bt) -> iso (runIdentity . bt) sa

-- rmap f (Exchange (sa, bt)) = Exchange (sa, f . bt)
case runExchange (lmap (*10) (Exchange (id, fmap (/10) . Identity))) of
  (sa, bt) -> iso (runIdentity . bt) sa

-- lmap f (Exchange (sa, bt)) = Exchange (sa . f, bt)
case runExchange (Exchange (id . (*10), fmap (/10) . Identity)) of
  (sa, bt) -> iso (runIdentity . bt) sa

case (id . (*10), fmap (/10) . Identity) of
  (sa, bt) -> iso (runIdentity . bt) sa

iso (runIdentity . fmap (/10) . Identity) (id . (*10))

iso (/10) (*10)
```

## Bazaar

### `both sell ("foo","bar")`

(Based on pre-3.8, 2012-12-26)

```haskell
both sell ("foo","bar")

-- both f (a,b) = (,) <$> f a <*> f b
(\f -> (,) <$> f "foo" <*> f "bar") sell

(,) <$> sell "foo" <*> sell "bar"

-- sell a = Bazaar (\f -> f a)
(,) <$> Bazaar (\f -> f "foo") <*> Bazaar (\f -> f "bar")

-- fmap f (Bazaar k) = Bazaar (fmap f . k)
Bazaar (fmap (,) . (\f -> f "foo")) <*> Bazaar (\f -> f "bar")

Bazaar (\f -> (,) <$> f "foo") <*> Bazaar (\f -> f "bar")

-- Bazaar mf <*> Bazaar ma = Bazaar $ \k -> mf k <*> ma k
Bazaar (\f -> (,) <$> f "foo" <*> f "bar")

Bazaar (\f -> both f ("foo","bar"))
```

### `partsOf' (both . both)`

See above for an evaluations of `both . both` and `l sell s`.

(Based on pre-3.8, 2012-12-26)

```haskell
partsOf' (both . both) f ((3,4),(5,6))

-- partsOf' l f s = outs b <$> f (ins b) where b = l sell s
let b = (both . both) sell ((3,4),(5,6))
in  outs b <$> f (ins b)

-- l sell s = Bazaar $ \g -> l g s
let b = Bazaar $ \g -> (both . both) g ((3,4),(5,6))
in  outs b <$> f (ins b)

-- ins = toListOf (flip runBazaar)
let b = Bazaar $ \g -> (both . both) g ((3,4),(5,6))
    inned = toListOf (flip runBazaar) b
in  outs b <$> f inned

-- toListOf l = getConst . l (\x -> Const [x])
let b = Bazaar $ \g -> (both . both) g ((3,4),(5,6))
    inned = getConst $ flip runBazaar (\x -> Const [x]) b
in  outs b <$> f inned

let b = Bazaar $ \g -> (both . both) g ((3,4),(5,6))
    inned = getConst $ runBazaar b (\x -> Const [x])
in  outs b <$> f inned

let b = Bazaar $ \g -> (both . both) g ((3,4),(5,6))
    inned = getConst $ (,) <$> ((,) <$> Const [3] <*> Const [4])
                           <*> ((,) <$> Const [5] <*> Const [6])
in  outs b <$> f inned

let b = Bazaar $ \g -> (both . both) g ((3,4),(5,6))
    inned = [3,4,5,6]
in  outs b <$> f inned

let b = Bazaar $ \g -> (both . both) g ((3,4),(5,6))
    outed = evalState (runBazaar b st)
    st _oldX = state (\(x:xs) -> (x, xs))
in  outed <$> f [3,4,5,6]

let outed = evalState $ (,) <$> ((,) <$> st 3 <*> st 4)
                            <*> ((,) <$> st 5 <*> st 6)
    st _oldX = state (\(x:xs) -> (x,xs))
in  outed <$> f [3,4,5,6]
```

```haskell
view (partsOf' (both . both)) ((3,4),(5,6))

getConst $ outed <$> Const [3,4,5,6]

[3,4,5,6]
```

```haskell
-- Better: over (partsOf' (both . both . mapped)) (*10) ((3,4),(5,6))
over (partsOf' (both . both)) (map (*10)) ((3,4),(5,6))

runIdentity $ outed <$> (Identity . map (*10)) [3,4,5,6]

((30,40),(50,60))
```
