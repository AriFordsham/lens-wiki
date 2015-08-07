`lens` didn't just come out of the aether fully formed, it was a result of many people having many ideas over a long time. Here are a few of the revelations, reading about them may help understand the ideas and concerns of the library.

**Jeremy Gibbons and Bruno C. d. S. Oliveira demonstrate that Traversals encode the Iterator pattern**

[The Essence of the Iterator Pattern](http://www.cs.ox.ac.uk/jeremy.gibbons/publications/iterator.pdf)

**Luke Palmer creates a pattern he calls _Accessors_ to ease stateful programming in Haskell**

(wayback)[Making Haskell nicer for game programming](http://web.archive.org/web/20080515203207/http://luqui.org/blog/archives/2007/07/26/making-haskell-nicer-for-game-programming/)

**Luke Palmer refines the notion of Accessors into something more like lenses**

(wayback)[Haskell State Accessors (second attempt: Composability)](http://web.archive.org/web/20071023064034/http://luqui.org/blog/archives/2007/08/05/haskell-state-accessors-second-attempt-composability/)

**Twan van Laarhoven comes up with a novel way to express lenses**

[CPS based functional references](http://www.twanvl.nl/blog/haskell/cps-functional-references)

**Russell O'Connor realises such lenses can support polymorphic update**

[Polymorphic Update with van Laarhoven Lenses](http://r6.ca/blog/20120623T104901Z.html)

**Edward Kmett realises that by changing the constraint on the Functor, it is possible to have lens-like things like Getters, Setters, and Traversals**

[Mirrored Lenses](http://comonad.com/reader/2012/mirrored-lenses/)