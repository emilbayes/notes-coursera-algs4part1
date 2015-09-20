Quick Union
===========

Lazy algorithm - Will defer work for as long as possible

Data structure
--------------

The following components: `{0 5 6} {1 2 7 8 9} {3 4}`

```java
// Indexes  0 1 2 3 4 5 6 7 8 9
int[] id = {0 1 1 3 3 0 5 2 7 1} // size N
```

Root of `i` is `id[id[id[...id[i]...]]]` until `i == id[i]`. This makes `union` and `connected` $$\mathcal{O}(N)$$

Implementation
--------------

```java
public class QuickUnionUF extends UF
{
    private int[] id;

    public QuickFindUF(int N)
    {
        id = new int[N];

        for (int i = 0; i < N; i++)
            id[i] = i;
    }

    public boolean connected(int p, int q)
    { return root(p) == root(q); }

    public void union(int p, int q)
    {
        int i = root(p);
        int j = root(q);
        id[i] = j;
    }

    private int root(int i)
    {
        while (i != id[i]) i = id[i];
        return i;
    }
}
```

Analysis
--------

| Operation  | $$\mathcal{O}(n)$$ |
|:-----------|:------------------:|
| Initialize |       $$N$$        |
| Union      | $$N$$<sup>†</sup>  |
| Find       |       $$N$$        |

<small>†: Includes the cost of finding roots</small>

**Deficiencies**: Trees can get tall, and find/`connected` is now expensive.
