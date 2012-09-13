Operators
=========

<table>
<thead>
<tr>
  <th>Description</th>
  <th>Combinator</th>
  <th>w/ Result</th>
  <th>State</th>
  <th>w/ Result</th>
  <th>Notes</th>
</tr>
</thead>
<tbody>
<tr>
  <td>view target(s)</td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Getter.html#v:view"><code>view</code></a>, <a href="http://ekmett.github.com/lens/Control-Lens-Getter.html#v:-94-."><code>^.</code></a></td>
  <td/>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Getter.html#v:use"><code>use</code></a></td>
  <td/>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Getter.html#v:query"><code>query</code></a> works like <a href="http://ekmett.github.com/lens/Control-Lens-Getter.html#v:use"><code>use</code></a> over a <code>MonadReader</code></td>
</tr>
<tr>
  <td>replace target(s)</td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:set"><code>set</code></a>, <a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:.-126-"><code>.~</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-60-.-126-"><code>&lt;.~</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:.-61-"><code>.=</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:assign"><code>assign</code></a>, <a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-60-.-61-"><code>&lt;.=</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Type.html#v:-60--60-.-126-"><code>&lt;&lt;.~</code> and
      <a href="http://ekmett.github.com/lens/Control-Lens-Type.html#v:-60--60-.-61-"><code>&lt;&lt;.=</code></a>
      return the old value</td>
</tr>
<tr>
  <td>update target(s)</td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:over"><code>over</code></a>, <a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-37--126-"><code>%~</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Type.html#v:-60--37--126-"><code>&lt;%~</code></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-37--61-"><code>%=</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Type.html#v:-60--37--61-"><code>&lt;%=</code></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Type.html#v:-60--60--37--126-"><code>&lt;&lt;%~</code> and
      <a href="http://ekmett.github.com/lens/Control-Lens-Type.html#v:-60--60--37--61-"><code>&lt;&lt;%=</code></a>
      return the old value</td>
</tr>
<tr>
  <td>update target(s)</td>
  <td><code>id</code>,<a href="http://ekmett.github.com/lens/Control-Lens-Traversal.html#v:traverseOf"><code>traverseOf</code></a>,<a href="http://ekmett.github.com/lens/Control-Lens-Type.html#v:-37--37--126-"><code>%%~</code></a></td>
  <td/>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Type.html#v:-37--37--61-"><code>%%=</code></a></td>
  <td/>
  <td>with an <code>Applicative</code> or auxillary result</td>
</tr>
<tr>
  <td>add to target(s)</td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-43--126-"><code>+~</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Type.html#v:-60--43--126-"><code>&lt;+~</code></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-43--61-"><code>+=</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Type.html#v:-60--43--61-"><code>&lt;+=</code></td>
</tr>
<tr>
  <td>subtract from target(s)</td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-45--126-"><code>-~</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Type.html#v:-60--45--126-"><code>&lt;-~</code></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-45--61-"><code>-=</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Type.html#v:-60--45--61-"><code>&lt;-=</code></td>
</tr>
<tr>
  <td>multiply target(s)</td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-42--126-"><code>*~</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Type.html#v:-60--42--126-"><code>&lt;*~</code></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-42--61-"><code>*=</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Type.html#v:-60--42--61-"><code>&lt;*=</code></td>
</tr>
<tr>
  <td>divide target(s)</td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-47--47--126-"><code>//~</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Type.html#v:-60--47--47--126-"><code>&lt;//~</code></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-47--47--61-"><code>//=</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Type.html#v:-60--47--47--61-"><code>&lt;//=</code></td>
</tr>
<tr>
  <td>exponentiate target(s)</td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-94--126-"><code>^~</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Type.html#v:-60--94--126-"><code>&lt;^~</code></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-94--61-"><code>^=</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Type.html#v:-60--94--61-"><code>&lt;^=</code></td>
  <td>to a non-negative <code>Integral</code> power</td>
</tr>
<tr>
  <td>exponentiate target(s)</td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-94--94--126-"><code>^^~</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Type.html#v:-60--94--94--126-"><code>&lt;^^~</code></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-94--94--61-"><code>^^=</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Type.html#v:-60--94--94--61-"><code>&lt;^^=</code></td>
  <td>to an <code>Integral</code> power</td>
</tr>
<tr>
  <td>exponentiate target(s)</td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-42--42--126-"><code>**~</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Type.html#v:-60--42--42--126-"><code>&lt;**~</code></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-42--42--61-"><code>**=</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Type.html#v:-60--42--42--61-"><code>&lt;**=</code></td>
  <td>to an arbitrary power</td>
</tr>
<tr>
  <td>logically or target(s)</td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-124--124--126-"><code>||~</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Type.html#v:-60--124--124--126-"><code>&lt;||~</code></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-124--124--61-"><code>||=</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Type.html#v:-60--124--124--61-"><code>&lt;||=</code></td>
</tr>
<tr>
  <td>logically and target(s)</td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-38--38--126-"><code>&amp;&amp;~</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Type.html#v:-60--38--38--126-"><code>&lt;&amp;&amp;~</code></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-38--38--61-"><code>&amp;&amp;=</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Type.html#v:-60--38--38--61-"><code>&lt;&amp;&amp;=</code></td>
</tr>
<tr>
  <td>bitwise or target(s)</td>
  <td><a href="http://ekmett.github.com/lens/Data-Bits-Lens.html#v:-124--126-"><code>|~</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Data-Bits-Lens.html#v:-60--124--126-"><code>&lt;|~</code></td>
  <td><a href="http://ekmett.github.com/lens/Data-Bits-Lens.html#v:-124--61-"><code>|=</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Data-Bits-Lens.html#v:-60--124--61-"><code>&lt;|=</code></td>
</tr>
<tr>
  <td>bitwise and target(s)</td>
  <td><a href="http://ekmett.github.com/lens/Data-Bits-Lens.html#v:-38--126-"><code>&amp;~</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Data-Bits-Lens.html#v:-60--38--126-"><code>&lt;&amp;~</code></td>
  <td><a href="http://ekmett.github.com/lens/Data-Bits-Lens.html#v:-38--61-"><code>&amp;=</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Data-Bits-Lens.html#v:-60--38--61-"><code>&lt;&amp;=</code></td>
</tr>
</tbody>
</table>