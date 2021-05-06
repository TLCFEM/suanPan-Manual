# NodeFacet

The `NodeFacet` constraint implements 3D node-facet (triangular) contact via Lagrangian multiplier method.

## Syntax

```
constraint NodeFacet (1) (2) (3) (4) (5)
# (1) int, unique constraint tag
# (2) int, master node i tag
# (3) int, master node j tag
# (4) int, master node k tag
# (5) int, slave node l tag
```