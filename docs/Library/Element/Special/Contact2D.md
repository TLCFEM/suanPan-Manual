# Contact2D

2D Node(Slave)-Line(Master) Contact

## Syntax

```
element Contact2D (1) (2) (3) [4]
# (1) int, unique element tag
# (2) int, master group tag
# (3) int, slave group tag
# [4] double, penalty factor
```

## Remarks

1. The contact algorithm is implemented via penalty function method as an element. The parameter `[4]` controls the magnitude of penalty.

2. The node-line contact detection is performed for all nodes defined in slave group and lines defined by nodes in master group. There are $n-1$ line segments for $n$ nodes in master group.

3. Master nodes can penetrate lines formed in slave group. To prevent this, define a new `Contact2D` element by swapping the order of slave and master groups.

4. Example usage can be seen in the [example](../../../Example/Structural/Contact/contact-between-beam-and-block.md).

5. 