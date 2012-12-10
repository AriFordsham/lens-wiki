I'm creating this page to help keep track of the branches in the current repository:

* `master` contains the current development HEAD.
* `gh-pages` is used to provide the HEAD haddocks at `http://ekmett.github.com/lens`.
* `3.6-maintenance` was created to help with backporting patches to 3.6 for `stackage` while development continued on 3.7. In time when we make a breaking change to 3.7, we'll go back and make a 3.7-maintenance and master will advance to be the working version for 3.8.
* `composerules` is a feature branch to explore whether or not it makes more sense to use RULES pragmas and a strict composition `(#)` rather than the existing practice of using `Control.Lens.Internal.Combinators` to get efficient core. We are unlikely to switch to this approach because it leads to a much more brittle series of steps that GHC would have to take to get the same core we generate now.

