Union-Find Applications
-----------------------

Many applications as listed in the [introduction](dynamic-connectivity.md#applications).

Percolation
-----------

Model for many physical systems.

Characteristics:

* $$N \times N$$ grid of sites
* Each site is $$\Pr(Blocked) = 1 - p$$
* System percolates iff `connected(top, bottom) == true`

Model Applications
------------------

| Model       | System     | Vacant Site | Occupied Site | Percolates    |
|:------------|:-----------|:------------|:--------------|:--------------|
| Electricity | Material   | Conductor   | Insulator     | Current Flows |
| Fluid       | Material   | Empty       | Bloced        | Porous        |
| Social      | Population | Person      | Empty         | Gossips       |

Analysis
--------

When large $$N$$, a sharp threshold, $$p^*$$, exists where the system percolates. The value of $$p^*$$ can be approximated using simulation.

Monte Carlo Simulation
----------------------

1. Initialize grid with all blocked sites
2. Unblock a single site at random
3. Does the system percolate?
    1. Yes, we're finished
    2. No, repeat 2.

$$p^* \approx \frac{Vacant}{Blocked}$$