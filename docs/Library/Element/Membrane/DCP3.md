# DCP3

Constant Strain Triangle With Crack Phase

* Number of Nodes: 3
* Number of DoFs: 3
* Zeo Mass Entries On Damage DoFs

## Reference

There are a lot of papers on the phase-field model.

* [10.1016/j.commatsci.2016.04.009](https://doi.org/10.1016/j.commatsci.2016.04.009)

## Syntax

```
element DCP3 (1) (2...4) (5) (6) (7) [8]
# (1) int, unique element tag
# (2...4) int, node i, j, k
# (5) int, material tag
# (6) double, characteristic length
# (7) double, energy release rate
# [8] double, element thickness, default: 1.0
```

## Remarks

* The third DoF is the damage variable. Boundary conditions can be assigned.
* The characteristic length `(7)` can be set to zero to automatically determine the value based on geometry.
