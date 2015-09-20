Quick Find
==========

Eager algorithm - will try to complete work as fast as possible, hence the $$\mathcal{O}(N)$$ run-time of he `union` operation.

Data structure
--------------

The following components: `{0 5 6} {1 2 7 8 9} {3 4}`

```java
// Indexes  0 1 2 3 4 5 6 7 8 9
int[] id = {0 1 1 3 3 0 0 1 1 1} // size N
```

$$p$$ and $$q$$ are connected if `id[p] == id[q]`

Algorithm
---------

### Find

```java
public boolean connected(int p, int q)
{
    return id[p] == id[q];
}
```

### Union

To merge two components containing $$p$$ and $$q$$ we have to recursively change all entries with `id[p]` to `id[q]` which might be $$N - 1$$ entries.

Implementation
--------------

```java
public class QuickFindUF extends UF
{
    private int[] id;

    public QuickFindUF(int N)
    {
        id = new int[N];

        for (int i = 0; i < N; i++)
            id[i] = i;
    }

    // QuickFind part
    public boolean connected(int p, int q)
    { return id[p] == id[q]; }

    // All pid's become qid's
    // 2N + 2 array accesses
    public void union(int p, int q)
    {
        int pid = id[p];
        int qid = id[q];
        for (int i = 0; i < id.length; i++)
            if (id[i] == pid) id[i] = qid;
    }
}
```

Analysis
--------

| Operation  | $$\mathcal{O}(n)$$ |
|:-----------|:------------------:|
| Initialize |       $$N$$        |
| Union      |       $$N$$        |
| Find       |       $$1$$        |

Side-note: $$\mathcal{O}(N^2)$$
-------------------------------

As computers get bigger, quadratic algorithms get slower. Since the 1950 it has almost always held true, that it takes about 1 sec to touch all of main memory. However as main memory gets bigger, quadratic algorithm's run-time grow faster than processing power by factor of $$N$$.
