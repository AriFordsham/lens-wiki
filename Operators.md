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
  <td>view</td>
  <td>^.</td>
  <td/>
  <td>use</td>
  <td/>
  <td><pre>query</pre> works on MonadReader</td>
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