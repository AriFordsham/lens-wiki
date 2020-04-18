`lens` didn't just come out of the aether fully formed, it was a result of many people having many ideas over a long time. Here are a few of the revelations, reading about them may help understand the ideas and concerns of the library.

**Jeremy Gibbons and Bruno C. d. S. Oliveira demonstrate that Traversals encode the Iterator pattern**

[The Essence of the Iterator Pattern](http://www.cs.ox.ac.uk/jeremy.gibbons/publications/iterator.pdf)

**Luke Palmer creates a pattern he calls _Accessors_ to ease stateful programming in Haskell**

[Making Haskell nicer for game programming](https://lukepalmer.wordpress.com/2007/07/26/making-haskell-nicer-for-game-programming/)

**Luke Palmer refines the notion of Accessors into something more like lenses**

[Haskell State Accessors (second attempt: Composability)](https://lukepalmer.wordpress.com/2007/08/05/haskell-state-accessors-second-attempt-composability/)

**Twan van Laarhoven comes up with a novel way to express lenses**

[CPS based functional references](http://www.twanvl.nl/blog/haskell/cps-functional-references)

**Russell O'Connor realises such lenses can support polymorphic update and noticed that they relate to his concept of a multilens (a concept which is now called a "traversal") if you vary the constraints**

[Polymorphic Update with van Laarhoven Lenses](http://r6.ca/blog/20120623T104901Z.html)

**Edward Kmett realises that you can put laws on the notion of polymorphic update**

[Mirrored Lenses](http://comonad.com/reader/2012/mirrored-lenses/)