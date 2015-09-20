Dynamic Connectivity
====================

Set of $$N$$ objects:

* **Union command** connect two objects
* **Find/connected query** is there a path connecting the two objects?

This is the goal of the dynamic connectivity problem, `union!(Node, Node)` and `connected?(Node, Node)`

Applications
------------

* Pixels
* Computers in network
* People in social network
* Transistors in chip
* Elements in mathematical set
* Variables in AST

Assumptions
-----------

A "connection" is an equivalence relation:

* Reflexive, $$(p, p)$$
* Symmetric, $$(p, q) \Leftrightarrow (q, p)$$
* Transitive, $$(p, q) \land (q, r) \Rightarrow (p, r)$$

API
---

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

Client Input
------------

```
10 // N
4 3 // Connect 4 and 3
3 8
6 5
...

```
