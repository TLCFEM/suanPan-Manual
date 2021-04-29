# Contact3D

3D Node(Slave)-Triangular Facet(Master) Contact Element

## Syntax

```
element Contact3D (1) (2) (3) [4]
# (1) int, unique element tag
# (2) int, master group tag
# (3) int, slave group tag
# [4] double, penalty factor
```

## Remarks

1. The contact algorithm is implemented via penalty function method as an element. The parameter `[4]` controls the magnitude of penalty.

2. The node-facet contact detection is performed for all nodes defined in slave group and facets defined by nodes in master group. There are $$n/3$$ facets for $$n$$ nodes in master group.