Operators
=========

<table>
<tr>
  <th>Description</th>
  <th>Named</th>
  <th>Functional</th>
  <th>w/ Result</th>
  <th>State</th>
  <th>w/ Result</th>
  <th>Notes</th>
</tr>
<tr>
  <td>view target(s)</td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Getter.html#v:view"><code>view</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Getter.html#v:-94-."><code>^.</code></a></td>
  <td/>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Getter.html#v:use"><code>use</code></a></td>
  <td/>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Getter.html#v:query"><code>query</code></a> works like <a href="http://ekmett.github.com/lens/Control-Lens-Getter.html#v:use"><code>use</code></a> over a <code>MonadReader</code></td>
</tr>
<tr>
  <td>replace target(s)</td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:set"><code>set</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:.-126-"><code>.~</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-60-.-126-"><code>&lt;.~</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:.-61-"><code>.=</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-60-.-61-"><code>&lt;.=</code></a></td>
</tr>
<tr>
  <td>update target(s)</td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:over"><code>over</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-37--126-"><code>%~</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Type.html#v:-60--37--126-"><code>&lt;%~</code></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-37--61-"><code>%=</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Type.html#v:-60--37--61-"><code>&lt;%=</code></td>
</tr>
<tr>
  <td>add to target(s)</td>
  <td/>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-43--126-"><code>+~</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Type.html#v:-60--43--126-"><code>&lt;+~</code></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-43--61-"><code>+=</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Type.html#v:-60--43--61-"><code>&lt;+=</code></td>
</tr>
<tr>
  <td>subtract from target(s)</td>
  <td/>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-45--126-"><code>-~</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Type.html#v:-60--45--126-"><code>&lt;-~</code></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-45--61-"><code>-=</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Type.html#v:-60--45--61-"><code>&lt;-=</code></td>
</tr>
<tr>
  <td>multiply target(s)</td>
  <td/>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-42--126-"><code>*~</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Type.html#v:-60--42--126-"><code>&lt;*~</code></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-42--61-"><code>*=</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Type.html#v:-60--42--61-"><code>&lt;*=</code></td>
</tr>
<tr>
  <td>divide target(s)</td>
  <td/>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-47--47--126-"><code>//~</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Type.html#v:-60--47--47--126-"><code>&lt;//~</code></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-47--47--61-"><code>//=</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Type.html#v:-60--47--47--61-"><code>&lt;//=</code></td>
</tr>
<tr>
  <td>exponentiate target(s)</td>
  <td/>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-94--126-"><code>^~</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Type.html#v:-60--94--126-"><code>&lt;^~</code></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-94--61-"><code>^=</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Type.html#v:-60--94--61-"><code>&lt;^=</code></td>
  <td>to a non-negative <code>Integral</code> power</td>
</tr>
<tr>
  <td>exponentiate target(s)</td>
  <td/>
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
</table>