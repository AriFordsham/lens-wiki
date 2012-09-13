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
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Getter.html#v:query"><code>query</code></a> works like <a href="http://ekmett.github.com/lens/Control-Lens-Getter.html#v:use"><code>use</code></a>, but over a <code>MonadReader</code></td>
</tr>
<tr>
  <td>replace target(s)</td>
  <td>set</td>
  <td>.~</td>
  <td>&lt;.~</td>
  <td>.=</td>
  <td>&lt;.=</td>
</tr>
<tr>
  <td>update target(s)</td>
  <td>over</td>
  <td>%~</td>
  <td>%~</td>
  <td>%=</td>
  <td>&lt;%=</td>
</tr>
<tr>
  <td>add to target(s)</td>
  <td/>
  <td>+~</td>
  <td>&lt;+~</td>
  <td>+=</td>
  <td>&lt;+=</td>
</tr>
</table>