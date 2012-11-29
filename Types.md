Types
=====

<table>
 <thead>
 <tr>
  <th>Type</th>
  <th>Definition</th>
  <th>Equivalent to</th>
  <th>Compare to</th>
  <th>Notes</th>
 </tr>
 </thead>
 <tbody>

 <tr>
  <td><code>Lens s t a b</code></td>
  <td><code>Functor f => (a -> f b) -> s -> f t</code></td>
  <td><code>s -> (a, b -> t)</code></td>
  <td></td>
  <td></td>
 </tr>

 <tr>
  <td><code>Traversal s t a b</code></td>
  <td><code>Applicative f => (a -> f b) -> s -> f t</code></td>
  <td><code>s -> (n:Nat, a^n, b^n -> t)</code></td>
  <td>Compare to <code>traverse</code></td>
  <td>Like <code>s -> ([a], [b] -> t)</code>, where the length of the lists must be equal</td>
 </tr>

 <tr>
  <td><code>Getter s a</code></td>
  <td><code>Gettable f => (a -> f a) -> s -> f s</code></td>
  <td><code>s -> a</code></td>
  <td></td>
  <td>a function</td>
 </tr>

 <tr>
  <td><code>Setter s a</code></td>
  <td><code>Settable f => (a -> f b) -> s -> f t</code></td>
  <td><code>(a -> b) -> s -> t</code></td>
  <td>Blah uncountable</td>
  <td><code>fmap</code></td>
 </tr>

 <tr>
  <td><code>Fold s a</code></td>
  <td><code>(Gettable f, Applicative f) => (a -> f a) -> s -> f s</code></td>
  <td><code>s -> [a] (= s -> (n:Nat, a^n)</code>)</td>
  <td></td>
  <td></td>
 </tr>

 <tr>
  <td><code>Iso s t a b</code></td>
  <td><code>(Isomorphic k, Functor f) => k (a -> f b) (s -> f t)</code></td>
  <td><code>(s -> a, b -> t)</code></td>
  <td></td>
  <td></td>
 </tr>

 <tr><th colspan="5">Indexed</th></tr>

 <tr>
  <td><code>IndexedLens i s t a b</code></td>
  <td><code>(Indexed i k, Functor f) => k (a -> f b) (s -> f t)</code></td>
  <td><code>s -> ((i,a), b -> t)</code></td>
  <td></td>
  <td></td>
 </tr>


 <tr>
  <td><code>IndexedTraversal i s t a b</code></td>
  <td><code>(Indexed i k, Applicative f) => k (a -> f b) (s -> f t)</code></td>
  <td><code>s -> (n:Nat, (i, a)^n, b^n -> t)</code></td>
  <td></td>
  <td></td>
 </tr>

 <tr>
  <td><code>IndexedGetter i s a</code></td>
  <td><code>(Indexed i k, Gettable f) => k (a -> f a) (s -> f s)</code></td>
  <td><code>s -> (i, a)</code></td>
  <td></td>
  <td></td>
 </tr>

 <tr>
  <td><code>IndexedSetter i s t a b</code></td>
  <td><code>(Indexed i k, Settable f) => k (a -> f b) (s -> f t)</code></td>
  <td><code>((i, a) -> b) -> s -> t</code></td>
  <td></td>
  <td></td>
 </tr>

 <tr>
  <td><code>IndexedFold i s a</code></td>
  <td><code>(Indexed i k, Gettable f, Applicative f) => k (a -> f a) (s -> f s)</code></td>
  <td><code>s -> [(i, a)]</code></td>
  <td></td>
  <td></td>
 </tr>

 <tr><th colspan="5">Monadic</th></tr>

 <tr>
  <td><code>Action m s a</code></td>
  <td><code>Effective m r f => (a -> f a) -> s -> f s</code></td>
  <td><code>s -> m a</code></td>
  <td></td>
  <td></td>
 </tr>

 <tr>
  <td><code>MonadicFold m s a</code></td>
  <td><code>(Effective m r f, Applicative f) => (a -> f a) -> s -> f s</code></td>
  <td><code>s -> m [a]</code></td>
  <td></td>
  <td></td>
 </tr>

 <tr><th colspan="5">Hypothetical</th></tr>

 <tr>
  <td><code>"PartialLens" s t a b</code></td>
  <td><code>Pointed f => (a -> f b) -> s -> f t</code></td>
  <td><code>s -> Either t (a, b -> t)</code></td>
  <td></td>
  <td>Might be closer to </td>
 </tr>

 <tr>
  <td><code>"NonEmptyTraversal" s t a b</code></td>
  <td><code>Apply f => (a -> f b) -> s -> f t</code></td>
  <td><code>s -> (n:Nat, a^(n+1), b^(n+1) -> t)</code></td>
  <td></td>
  <td></td>
 </tr>

 </tbody>
</table>

Notes:

* `a^n` means "a list of `n` `a`s".
* `Nat` really means `Conat` (`Nat` + ∞), since a `Traversal`/`Fold` can be infinite.
