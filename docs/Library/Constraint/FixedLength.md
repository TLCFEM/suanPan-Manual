# FixedLength

The Lagrange multiplier method can be used to define constraints. Extra storage is required for the so called auxiliary bordered stiffness matrix. The constraint is satisfied exactly.

The `FixedLength2D` and `FixedLength3D` constraints behave similar to the `Tie` element, the nonlinear constraint is implemented via Lagrange multiplier.

It shall not be used with global damping models in dynamic analaysis.

## Syntax

```
fixedlength2d (1) (2) (3) (4)
fixedlength3d (1) (2) (3) (4)
# (1) int, unique constraint tag
# (2) int, amplitude tag, can be zero
# (3) int, node tag
# (4) int, node tag
```