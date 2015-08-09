Operators
=========

<table>
<thead>
<tr>
  <th>Combinator(s)</th>
  <th>w/ Result</th>
  <th>Stateful</th>
  <th>w/ Result</th>
  <th>Notes</th>
</tr>
</thead>
<tbody>
<tr><th colspan=5><a href="http://ekmett.github.com/lens/Control-Lens.html">Control.Lens (Getting)</a></th></tr>
<tr>
  <td rowspan=1>
      <a href="http://ekmett.github.com/lens/Control-Lens-Getter.html#v:view">
        <code>view</code></a>
      /
      <a href="http://ekmett.github.com/lens/Control-Lens-Getter.html#v:-94-.">
       <code>^.</code></a>
  </td>
  <td/>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Getter.html#v:use"><code>use</code></a>
  <td/>
  <td>View single target, or fold multiple monoidal targets.
         <a href="http://ekmett.github.com/lens/Control-Lens-Getter.html#v:use"><code>use</code></a> 
         works just like
         <a href="http://ekmett.github.com/lens/Control-Lens-Getter.html#v:view"><code>view</code></a> 
         except in a MonadState instead of a MonadReader.  
  </td>
</tr>
<tr>
  <td colspan=5>
<pre>
view _2 ("hello","world") == "world" 
view _2 ("hello","world","yeeha") == "world"
("hello","world") ^. _2 == "world"
[Sum 3, Sum 4] ^. each == Sum 7  
</pre>
  </td>
</tr>

<tr>
  <td>
      <a href="http://ekmett.github.com/lens/Control-Lens-Getter.html#v:views"><code>views</code></a>      
  </td>
  <td/>
  <td>
      <a href="http://ekmett.github.com/lens/Control-Lens-Getter.html#v:uses"><code>uses</code></a></td>
  <td/>
  <td>View/fold after applying a function. </td>
</tr>
<tr>
  <td colspan=5>
<pre>
views _2 (map toUpper) ("hello","world") == "WORLD"
views each (Sum . succ . getSum) [Sum 1, Sum 2] == Sum 5
</pre>
  </td>
</tr>

<tr>
  <td>
     <a href="http://ekmett.github.com/lens/Control-Lens-Getter.html#v:to">
       <code>to</code></a>
  </td>
  <td/>
  <td>
  <td/>
  <td>
      Build a getter from a plain function.
  </td>
</tr>
<tr>
  <td colspan=5>
<pre>
view (to snd) ("hello","world") == "world"
</pre>
  </td>
</tr>
<tr/> <!-- to make table coloring match first section -->
<tr><th colspan=5><a href="http://ekmett.github.com/lens/Control-Lens.html">Control.Lens (Setting)</a></th></tr>
<tr>
  <td>
      <a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:set">
        <code>set</code></a> 
      / 
      <a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:.-126-">
        <code>.~</code></a>
  </td>
  <td>
      <a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-60-.-126-">
        <code>&lt;.~</code></a>
  </td>
  <td>
      <a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:assign">
        <code>assign</code></a> 
      / 
      <a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:.-61-">
        <code>.=</code></a>
  </td>
  <td>
      <a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-60-.-61-">
        <code>&lt;.=</code></a>
  </td>
  <td>Replace target(s) with value.       
      <a href="http://ekmett.github.com/lens/Control-Lens-Lens.html#v:-60--60-.-126-">
        <code> &lt;&lt;.~</code> </a> and
      <a href="http://ekmett.github.com/lens/Control-Lens-Lens.html#v:-60--60-.-61-">
        <code> &lt;&lt;.= </code></a>
      return the old value.
  </td>
</tr>
<tr>
  <td colspan=5>
<pre>
set _2 "pluto" ("hello","world") == ("hello","pluto")
set each 0 [1,2] == [0,0]
</pre>
  </td>
</tr>

<tr>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:over"><code>over</code></a> / <a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-37--126-"><code>%~</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Lens.html#v:-60--37--126-"><code>&lt;%~</code></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-37--61-"><code>%=</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Lens.html#v:-60--37--61-"><code>&lt;%=</code></td>
  <td>Replace target(s) by applying function. 
      <a href="http://ekmett.github.com/lens/Control-Lens-Lens.html#v:-60--60--37--126-">
        <code>&lt;&lt;%~</code></a> and
      <a href="http://ekmett.github.com/lens/Control-Lens-Lens.html#v:-60--60--37--61-">
        <code>&lt;&lt;%=</code></a>
      return the old value
  </td>
</tr>
<tr>
  <td colspan=5>
<pre>
over _2 (map toUpper) ("hello","world") == ("hello", "WORLD")
over each (map toUpper) ["hello","world"] == ("HELLO", "WORLD")
</pre>
  </td>
</tr>

<tr>
  <td>
      <a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:setting">
        <code>setting</code></a>
      , 
      <a href="http://ekmett.github.com/lens/Data-Data-Lens.html#v:upon">
        <code>upon</code></a>
  </td>
  <td>
  </td>
  <td>
  </td>
  <td>

  </td>
  <td>Build a setter from a map-like function.
  </td>
</tr>
<tr>
  <td colspan=5>
<pre>
  let updateSnd f (x,y) = (x, f y) 
  set (setting updateSnd) "pluto" ("hello","world") == ("hello","pluto")
</pre>
  </td>
</tr>

<tr>
  <td>
      <code>id</code> / 
      <a href="http://ekmett.github.com/lens/Control-Lens-Lens.html#v:-37--37--126-"><code>%%~</code></a> /
      <a href="http://ekmett.github.com/lens/Control-Lens-Traversal.html#v:traverseOf"> 
        <code> traverseOf </code></a> 
      </td>
  <td/><td>
      <a href="http://ekmett.github.com/lens/Control-Lens-Lens.html#v:-37--37--61-">
        <code>%%=</code></a>   
  </td><td/>
  <td>Update target(s) with an Applicative or auxiliary result.<br/>
      <code>traverseOf</code> can replace <code>(%%~)</code> but is less general. <code>traverseOf</code> applies to Over type, whereas <code>(%%~)</code> applies to <code>Optical</code> type. <code>Over p</code> is equivalent to <code>Optical p (->)</code>
  </td>
</tr>
<tr>
  <td colspan=5>
<pre>
let succIfEven x = if even x then Just . succ $ x else Nothing
(each %%~ succIfEven) [2,4,6] == Just [3,5,7]
(_2 %%~ succIfEven) (2,4,6) == Just (2,5,6)
</pre>
  </td>
</tr>


<tr/> <!-- to make table coloring match first section -->
<tr><th colspan=5>
    <a href="http://ekmett.github.com/lens/Control-Lens-Fold.html">Control.Lens.Fold</a>
    /     <a href="http://ekmett.github.com/lens/Control-Lens-Prism.html">Control.Lens.Prism</a>
</th></tr>

<tr>
  <td>
      <a href="http://ekmett.github.com/lens/Control-Lens-Prism.html#v:review">
        <code>review</code></a> 
  </td>
  <td/>
  <td/><td/>
  <td>
       Choose a branch of a sum type
  </td>
</tr>
<tr>
  <td colspan=5>
<pre>
  review _Right 1 == Right 1
</pre>
  </td>
</tr>

<tr>
  <td>
      <a href="http://ekmett.github.com/lens/Control-Lens-Fold.html#v:preview">
        <code>preview</code></a>  / 
      <a href="http://ekmett.github.com/lens/Control-Lens-Fold.html#v:-94--63-">
        <code>^?</code></a> /
      <a href="http://ekmett.github.com/lens/Control-Lens-Fold.html#v:firstOf">
        <code>firstOf</code></a>
  </td>
  <td/>
  <td/><td/>
  <td>
       Get the first target or Nothing if there are no targets.
  </td>
</tr>
<tr>
  <td colspan=5>
<pre>
preview _Right (Right 1) == Just 1
preview _Right Nothing   == Nothing
preview traverse []      == Nothing
preview traverse [1]     == Just 1 
</pre>
  </td>
</tr>

<tr>
  <td>
      <a href="http://ekmett.github.com/lens/Control-Lens-Prism.html#v:previews">
        <code>previews</code></a> 
  </td>
  <td/>
  <td/><td/>
  <td>
       preview after applying a function
  </td>
</tr>
<tr>
  <td colspan=5>
<pre>
previews _Right (+1) (Right 1) == Just 2
</pre>
  </td>
</tr>

<tr>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Fold.html#v:toListOf"><code>toListOf</code> / <a href="http://ekmett.github.com/lens/Control-Lens-Fold.html#v:-94-.."><code>^..</code></a></td>
  <td/><td/><td/>
  <td>
    Return a list of the target(s)
  </td>
</tr>
<tr>
  <td colspan=5>
<pre>
toListOf traverse ('a','b')  == "b"
toListOf each ('a','b')  == "ab"
</pre>
  </td>
</tr>


<tr><th colspan=5><a href="http://ekmett.github.com/lens/Control-Lens.html">Control.Lens (Getting Indexed)</a></th></tr>

<tr>
  <td>
    <a href="http://ekmett.github.io/lens/Control-Lens-Getter.html#v:iview">
      <code>iview</code></a> / 
    <code>^@.</code>
  </td>
  <td>
    
  </td>
  <td> <code> iuse </code>
  </td>

  <td> 
  </td>
  <td>
    View the index and value of an IndexedGetter or IndexedLens.
    </p>
</td>
</tr>
<tr>
  <td colspan=5>
<pre>
let mapHW = Data.Map.fromList [(1,"hello"),(2,"world")]
mapHW ^@. iat 2 == (2,Just "world")
</pre>
  </td>
</tr>



<tr>
  <td>
    <a href="http://ekmett.github.io/lens/Control-Lens-Getter.html#v:iviews">
      <code>iviews</code></a> 
  </td>
  <td>
    
  </td>
  <td> <code> iuses </code>
  </td>

  <td> 
  </td>
  <td>    View a function of the index and value of an IndexedGetter into the current environment.
</td>
</tr>

<tr><th colspan=5><a href="http://ekmett.github.com/lens/Control-Lens.html">Control.Lens (Setting Indexed)</a></th></tr>


<tr>
  <td>
    <a href="http://ekmett.github.io/lens/Control-Lens-Setter.html#v:iset">
      <code>iset</code></a> / 
    <code>.@~</code>
  </td>
  <td>
    
  </td>
  <td>
    <code>.@=</code>
  </td>
  <td>
    
  </td>
  <td>Set target(s) with access to the index.</td>
</tr>

<tr>
  <td>
    <a href="http://ekmett.github.io/lens/Control-Lens-Setter.html#v:iover">
      <code>iover</code></a> / 
    <a href="http://ekmett.github.com/lens/Control-Lens-Operators.html#v:-37--64--126-">
      <code>%@~</code></a>
  </td>
  <td>
    <a href="http://ekmett.github.com/lens/Control-Lens-Operators.html#v:-60--37--64--126-">
      <code>&lt;%@~</code>
  </td>
  <td>
    <a href="http://ekmett.github.com/lens/Control-Lens-Operators.html#v:-37--64--61-">
      <code>%@=</code></a>
  </td>
  <td>
    <a href="http://ekmett.github.com/lens/Control-Lens-Operators.html#v:-60--37--64--61-">
      <code>&lt;%@=</code>
  </td>
  <td>Update target(s) with access to the index.</td>
</tr>
<tr>
  <td>
    <a href="http://ekmett.github.com/lens/Control-Lens-Indexed.html#v:withIndex">
      <code>withIndex</code></a>, 
    <a href="http://ekmett.github.com/lens/Control-Lens-IndexedTraversal.html#v:itraverseOf">
      <code>itraverseOf</code></a>, 
    <a href="http://ekmett.github.com/lens/Control-Lens-Operators.html#v:-37--37--64--126-">
      <code>%%@~</code></a>
  </td>
  <td/>
  <td>
    <a href="http://ekmett.github.com/lens/Control-Lens-Operators.html#v:-37--37--64--61-">
      <code>%%@=</code></a></td>
  <td/>
  <td>
      Update target(s) with an <code>Applicative</code> or auxiliary result with access to the index.
  </td>
</tr>
<tr><th colspan=5><a href="http://ekmett.github.com/lens/Control-Lens.html">Control.Lens (math)</a></th></tr>
<tr>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-43--126-"><code>+~</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Lens.html#v:-60--43--126-"><code>&lt;+~</code></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-43--61-"><code>+=</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Lens.html#v:-60--43--61-"><code>&lt;+=</code></td>
  <td>Add to target(s)</td>
</tr
<tr>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-45--126-"><code>-~</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Lens.html#v:-60--45--126-"><code>&lt;-~</code></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-45--61-"><code>-=</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Lens.html#v:-60--45--61-"><code>&lt;-=</code></td>
  <td>Subtract from target(s)</td>
</tr>
<tr>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-42--126-"><code>*~</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Lens.html#v:-60--42--126-"><code>&lt;*~</code></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-42--61-"><code>*=</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Lens.html#v:-60--42--61-"><code>&lt;*=</code></td>
  <td>Multiply target(s)</td>
</tr>
<tr>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-47--47--126-"><code>//~</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Lens.html#v:-60--47--47--126-"><code>&lt;//~</code></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-47--47--61-"><code>//=</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Lens.html#v:-60--47--47--61-"><code>&lt;//=</code></td>
  <td>Divide target(s)</td>
</tr>
<tr>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-94--126-"><code>^~</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Lens.html#v:-60--94--126-"><code>&lt;^~</code></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-94--61-"><code>^=</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Lens.html#v:-60--94--61-"><code>&lt;^=</code></td>
  <td>Raise target(s) to a non-negative <code>Integral</code> power</td>
</tr>
<tr>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-94--94--126-"><code>^^~</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Lens.html#v:-60--94--94--126-"><code>&lt;^^~</code></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-94--94--61-"><code>^^=</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Lens.html#v:-60--94--94--61-"><code>&lt;^^=</code></td>
  <td>Raise target(s) to an <code>Integral</code> power</td>
</tr>
<tr>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-42--42--126-"><code>**~</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Lens.html#v:-60--42--42--126-"><code>&lt;**~</code></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-42--42--61-"><code>**=</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Lens.html#v:-60--42--42--61-"><code>&lt;**=</code></td>
  <td>Raise target(s) to an arbitrary power</td>
</tr>
<tr><th colspan=5><a href="http://ekmett.github.com/lens/Control-Lens.html">Control.Lens (logic)</a></th></tr>
<tr>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-124--124--126-"><code>||~</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Lens.html#v:-60--124--124--126-"><code>&lt;||~</code></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-124--124--61-"><code>||=</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Lens.html#v:-60--124--124--61-"><code>&lt;||=</code></td>
  <td>Logically or target(s)</td>
</tr>
<tr>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-38--38--126-"><code>&amp;&amp;~</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Lens.html#v:-60--38--38--126-"><code>&lt;&amp;&amp;~</code></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-38--38--61-"><code>&amp;&amp;=</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Lens.html#v:-60--38--38--61-"><code>&lt;&amp;&amp;=</code></td>
  <td>Logically and target(s)</td>
</tr>
<tr>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-60--62--126-"><code>&lt;&gt;~</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Lens.html#v:-60--60--62--126-"><code>&lt;&lt;&gt;~</code></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Setter.html#v:-60--62--61-"><code>&lt;&gt;=</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Control-Lens-Lens.html#v:-60--60--62--61-"><code>&lt;&lt;&gt;=</code></td>
  <td><code>mappend</code> to the target monoidal value(s)</td>
</tr>



<tr><th colspan=5><a href="http://ekmett.github.com/lens/Data-Bits-Lens.html">Data.Bits.Lens</a></th></tr>
<tr>
  <td><a href="http://ekmett.github.com/lens/Data-Bits-Lens.html#v:-124--126-"><code>.|.~</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Data-Bits-Lens.html#v:-60--124--126-"><code>&lt;.|.~</code></td>
  <td><a href="http://ekmett.github.com/lens/Data-Bits-Lens.html#v:-124--61-"><code>.|.=</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Data-Bits-Lens.html#v:-60--124--61-"><code>&lt;.|.=</code></td>
  <td>Bitwise or target(s)</td>
</tr>
<tr>
  <td><a href="http://ekmett.github.com/lens/Data-Bits-Lens.html#v:-38--126-"><code>.&amp;.~</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Data-Bits-Lens.html#v:-60--38--126-"><code>&lt;.&amp;.~</code></td>
  <td><a href="http://ekmett.github.com/lens/Data-Bits-Lens.html#v:-38--61-"><code>.&amp;.=</code></a></td>
  <td><a href="http://ekmett.github.com/lens/Data-Bits-Lens.html#v:-60--38--61-"><code>&lt;.&amp;.=</code></td>
  <td>Bitwise and target(s)</td>
</tr>
<tr><th colspan=5><a href="http://ekmett.github.com/lens/System-FilePath-Lens.html">System.FilePath.Lens</a></th></tr>
<tr>
  <td><a href="http://ekmett.github.com/lens/System-FilePath-Lens.html#v:-60--47--62--126-"><code>&lt;/&gt;~</code></a></td>
  <td><a href="http://ekmett.github.com/lens/System-FilePath-Lens.html#v:-60--60--47--62--126-"><code>&lt;&lt;/&gt;~</code></td>
  <td><a href="http://ekmett.github.com/lens/System-FilePath-Lens.html#v:-60--47--62--61-"><code>&lt;/&gt;=</code></a></td>
  <td><a href="http://ekmett.github.com/lens/System-FilePath-Lens.html#v:-60--60--47--62--61-"><code>&lt;&lt;/&gt;=</code></td>
  <td>Append a relative path to a <code>FilePath</code></td>
</tr>
<tr>
  <td><a href="http://ekmett.github.com/lens/System-FilePath-Lens.html#v:-60-.-62--126-"><code>&lt;.&gt;~</code></a></td>
  <td><a href="http://ekmett.github.com/lens/System-FilePath-Lens.html#v:-60--60-.-62--126-"><code>&lt;&lt;.&gt;~</code></td>
  <td><a href="http://ekmett.github.com/lens/System-FilePath-Lens.html#v:-60-.-62--61-"><code>&lt;.&gt;=</code></a></td>
  <td><a href="http://ekmett.github.com/lens/System-FilePath-Lens.html#v:-60--60-.-62--61-"><code>&lt;&lt;.&gt;=</code></td>
  <td>Append a file extension to a <code>FilePath</code></td>
</tr>
</tbody>
</table>