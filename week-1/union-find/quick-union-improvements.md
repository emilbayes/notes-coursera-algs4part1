Quick-Union Improvements
========================

Weighting
---------

A technique to avoid tall trees by keeping track of the size of trees, and when merging, attaching the smaller tree to the root of the taller tree. This means that a subtree will only get one level deeper at each `union`. You could also weigh by "height", "rank" or some other measure with total order.

### Data Structure

Keep extra `int[] sz` array that maintains the size of the subtree for `ìd[i]` and `sz[j]` where `i == j`. `sz` is initialized to be an array of all `1`s.

### Algorithm

#### Find

Identical to [Quick-Union](quick-union.md#implementation)

#### Union

```java
public void union(int p, int q)
{
    int i = root(p);
    int j = root(q);

    if (i == j) return;
    if (sz[i] < sz[j]) { id[i] = j; sz[j] += sz[i]; }
    else               { id[j] = i; sz[i] += sz[j]; }
}
```

### Analysis

| Operation  |   $$\mathcal{O}(n)$$   |
|:-----------|:----------------------:|
| Initialize |         $$N$$          |
| Union      | $$\lg(N)$$<sup>†</sup> |
| Find       |       $$\lg(N)$$       |

#### Proof of Find run-time

**Proposition** Depth of any node $$x$$ is at most $$\lg(N)$$

**Proof** Depth of $$x$$ increases by at most one for each union.
Since the depth of $$x$$ only increase if it's merged with a tree that's larger or equal to the tree $$x$$ is in, the size of the new tree is at least double the size of $$x$$ original tree, since $$\|T_y\| \geq \|T_x\|$$. Since there's N nodes available, this doubling can happen at most $$log_2(N) \equiv \lg(N)$$ times.

<small>Law of logarithms: $$N = 2^a \Rightarrow \lg(N) = a$$</small>

Path Compression
----------------

When finding the path for $$p$$, we change the id of each of the nodes in the path to point to their root.

#### Algorithm

```java
private int root(int i)
{
    while (i != id[i])
    {
        id[i] = id[id[i]]; // <- Only change!
        i = id[i];
    }
    return i;
}
```

This will keep the tree almost completely flat as nodes will be updated on calls to `union` and `connected`.

Analysis - Combined
-------------------

| Operation  | $$\mathcal{O}(n)$$ |
|:-----------|:------------------:|
| Initialize |       $$N$$        |
| Union      |    $$\lg^*(N)$$    |
| Find       |    $$\lg^*(N)$$    |

In practice the Weighted Quick-Union with Path Compression will have linear run-time for all operations, however in theory it is not quite linear.

It has been proven that there is no linear time algorithm for Union-Find.
