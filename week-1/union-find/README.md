Union-Find
==========

Steps to develop usable algorithms:

1. Model the problem
2. Find an algorithm to solve it
3. Fast enough? Fits in memory?
4. If not, figure out why
5. Find a way to address the problem
6. Iterate until satisfied

Dynamic Connectivity
--------------------

Set of $$N$$ objects:

* **Union command** connect two objects
* **Find/connect query** is there a path connecting the two objects

This is the goal of the dynamic connectivity problem, `union!(Node, Node)` and `connected?(Node, Node)`

###Applications

* Pixels
* Computers in network
* Friends in social network
* Transistors in chip
* Elements in mathematical set
* Variables in AST

###Assumptions

A "connection" is an equivalence relation:

* Reflexive, $$(p, p)$$
* Symmetric, $$(p, q) \Leftrightarrow (q, p)$$
* Transitive, $$(p, q) \land (q, r) \Rightarrow (p, r)$$

###API

Requirements:

* $$N$$, the number of objects, can be **huge**
* $$M$$, the number of operations, can be **huge**
* `union` and `connected` can be intermixed

```java
public class UF
{
    // Initialize data structure with N objects (0 to N - 1)
    void UF(int N)

    // Add connection between p and q
    void union(int p, int q)

    // Query connection between p and q
    boolean connected(int p, int q)

    // Component identifier for p (can be as large as N - 1 if all nodes are disconnected)
    int find(int p)

    // Number of components
    int count()
}
```

###Client Input

```
10 // N
4 3 // Connect 4 and 3
3 8
6 5
...

```

Quick Find
----------

Eager algorithm - will try to complete work as fast as possible, hence the $$\mathcal{O}(N)$$ run-time of he `union` operation.

###Data structure

The following components: `{0 5 6} {1 2 7 8 9} {3 4}`

```java
// Indexes  0 1 2 3 4 5 6 7 8 9
int[] id = {0 1 1 3 3 0 0 1 1 1} // size N
```

$$p$$ and $$q$$ are connected if `id[p] == id[q]`

###Algorithm

####Find

```java
public boolean connected(int p, int q)
{
    return id[p] == id[q];
}
```

####Connected

To merge two components containing $$p$$ and $$q$$ we have to recursively change all entries with `id[p]` to `id[q]` which might be $$N - 1$$ entries.

###Implementation

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

###Analysis

| Operation  | $$\mathcal{O}(n)$$ |
|:-----------|:------------------:|
| Initialize |       $$N$$        |
| Union      |       $$N$$        |
| Find       |       $$1$$        |

###Side-note: $$\mathcal{O}(N^2)$$
As computers get bigger, quadratic algorithms get slower. Since the 1950 it has almost always held true, that it takes about 1 sec to touch all of main memory. However as main memory gets bigger, quadratic algorithm's run-time grow faster than processing power by factor of $$N$$.


Quick Union
-----------

Lazy algorithm - Will defer work for as long as possible

###Data structure

The following components: `{0 5 6} {1 2 7 8 9} {3 4}`

```java
// Indexes  0 1 2 3 4 5 6 7 8 9
int[] id = {0 1 1 3 3 0 5 2 7 1} // size N
```

Root of `i` is `id[id[id[...id[i]...]]]` until `i == id[i]`. This makes `union` $$\mathcal{O}(N)$$ and `connected` $$\mathcal{O}(N)$$

###Implementation

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

###Analysis

| Operation  | $$\mathcal{O}(n)$$ |
|:-----------|:------------------:|
| Initialize |       $$N$$        |
| Union      |       $$N*$$       |
| Find       |       $$N$$        |

\* Includes the cost of finding roots

**Deficiencies**: Trees can get tall, and find is now expensive.


Quick-Union Improvements
------------------------

### Weighting
A technique to avoid tall trees by keeping track of the size of trees, and when merging, attaching the smaller tree to the root of the taller tree. This means that a subtree will only get one level deeper at each `union`. You could also weigh by "height", "rank" or some other measure with total order.

####Algorithm

#####Find
Identical to [quick-union](#implementation2)

Union-Find Applications
-----------------------
