For whenever you need to translate an expanded type signature into the correct Lens/Prism/Traversal/&c. synonym.

Given a general type signature like `p a (f b) -> p s (f t)` the following c

| Synonym               | Constraints     |
|-----------------------|-----------------|
| **Equality** s t a b  |                 |
|                       |                 |
| **Iso** s t a b       | Profunctor p    |
|                       | Functor f       |
|                       |                 |
| **Prism** s t a b     | Choice p        |
|                       | Applicative f   |
|                       |                 |
| **Review** s t a b    | Profunctor p    |
|                       | Bifunctor p     |
|                       | Settable f      |
|                       |                 |
| **Lens** s t a b      | (->) ~ p        |
|                       | Functor f       |
|                       |                 |
| **Traversal** s t a b | (->) ~ p        |
|                       | Applicative f   |
|                       |                 |
| **Setter** s t a b    | (->) ~ p        |
|                       | Settable f      |
|                       |                 |
| **Action** m s a      | (->) ~ p        |
|                       | b ~ a           |
|                       | t ~ s           |
|                       | Effective m r f |
|                       |                 |
| **MonadicFold** m s a | (->) ~ p        |
|                       | b ~ a           |
|                       | t ~ s           |
|                       | Effective m r f |
|                       | Applicative f   |
|                       |                 |
| **Getter** s a        | (->) ~ p        |
|                       | b ~ a           |
|                       | t ~ s           |
|                       | Contravariant f |
|                       | Functor f       |
|                       |                 |
|                       |                 |
| **Fold** s a          | (->) ~ p        |
|                       | b ~ a           |
|                       | t ~ s           |
|                       | Contravariant f |
|                       | Applicative f   |