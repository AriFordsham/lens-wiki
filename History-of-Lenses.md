`lens` didn't just come out of the aether fully formed, it was a result of many people having many ideas over a long time. Here are a few of the revelations, reading about them may help understand the ideas and concerns of the library.

### Jeremy Gibbons and Bruno C. d. S. Oliveira demonstrate that Traversals encode the Iterator pattern
[The Essence of the Iterator Pattern](http://www.cs.ox.ac.uk/jeremy.gibbons/publications/iterator.pdf)

### Twan van Laarhoven comes up with a novel way to express lenses
[CPS based functional references](http://www.twanvl.nl/blog/haskell/cps-functional-references)

### Russel O'Connor realises such lenses can support polymorphic update
[Polymorphic Update with van Laarhoven Lenses](http://r6.ca/blog/20120623T104901Z.html)

### Edward Kmett realises that by changing the constraint on the Functor, it is possible to have lens-like things like Getters, Setters, and Traversals
[Mirrored Lenses](http://comonad.com/reader/2012/mirrored-lenses/)