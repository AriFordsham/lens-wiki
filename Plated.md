# Plated

Included in this package is a port of Neil Mitchell's `uniplate`, adapted to work over a `Traversal` instead.

The class is `Plated`, rather than `Uniplate` to avoid direct conflict as the instances are defined rather differently.

Benchmarks show that a hand-constructed Traversal-based `plate` and a hand constructed `uniplate` have effectively the same performance (within the margin of error), but the `Data` definitions used herein are about 50% faster than those supplied by `uniplate`, primarily because it doesn't have to build up and tear down an explicit structure representing the shape.