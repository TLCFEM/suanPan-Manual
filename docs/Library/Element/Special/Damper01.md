# Damper01

Viscous Damper

This `Damper01` is a simple viscous damper. This damper uses **displacement** and **velocity** as inputs, not strain and strain rate.

* Number of Nodes: 2
* Number of DoFs: 2

## Syntax

```
element Damper01 (1) (2) (3) (4)
# (1) int, unique tag
# (2) int, node i
# (3) int, node j
# (4) int, material tag of associated viscosity material
```