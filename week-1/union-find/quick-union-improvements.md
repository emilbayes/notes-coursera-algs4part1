Quick-Union Improvements
========================

Weighting
---------

A technique to avoid tall trees by keeping track of the size of trees, and when merging, attaching the smaller tree to the root of the taller tree. This means that a subtree will only get one level deeper at each `union`. You could also weigh by "height", "rank" or some other measure with total order.

### Algorithm

#### Find
Identical to [quick-union](quick-union.md#implementation)

#### Union
