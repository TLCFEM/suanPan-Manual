# load

Currently, concentrated node based loads can be applied in terms of force, displacement and acceleration.

Given that all external forces are eventually converted to concentrated nodal forces, commands for distributed load patterns may not be available in the foreseeable future.

## Syntax

### Nodal Load

To apply loads on nodes,

```
cload (1) (2) (3) (4) (5 ...)
displacement (1) (2) (3) (4) (5 ...)
# (1) int, unique tag
# (2) int, amplitude tag, 0 to use a default `Ramp` amplitude
# (3) double, reference magnitude
# (4) int, dof tag
# (5 ...) int, node tags

acceleration (1) (2) (3) (4) [5 ...]
# (1) int, unique tag
# (2) int, amplitude tag, 0 to use a default `Ramp` amplitude
# (3) double, reference magnitude
# (4) int, dof tag
# [5 ...] int, node tags
```

### Body Force

Some elements support body force. Indications are given for elements that support body force.

```
bodyforce (1) (2) (3) (4) (5 ...)
# (1) int, unique tag
# (2) int, amplitude tag, 0 to use a default `Ramp` amplitude
# (3) double, reference magnitude
# (4) int, dof tag
# (5 ...) int, element tags
```

### Load Applied To Node/Element Groups

To apply loads on groups,

```
# on node groups
groupcload (1) (2) (3) (4) (5 ...)
groupdisplacement (1) (2) (3) (4) (5 ...)
# on element groups
groupbodyforce (1) (2) (3) (4) (5 ...)
# (1) int, unique tag
# (2) int, amplitude tag, 0 to use a default `Ramp` amplitude
# (3) double, reference magnitude
# (4) int, dof tag
# (5 ...) int, group tags
```

### Support Excitation

For response history analysis, sometimes it is necessary to apply excitations on supports. The multi-support excitation is automatically supported if analysts assign different excitations to different supports.

```
supportdisplacement (1) (2) (3) (4) (5 ...)
supportvelocity (1) (2) (3) (4) (5 ...)
supportacceleration (1) (2) (3) (4) (5 ...)
# (1) int, unique tag
# (2) int, amplitude tag, 0 to use a default `Ramp` amplitude
# (3) double, reference magnitude
# (4) int, dof tag
# (5 ...) int, node tags
```

## Remarks

1. The `acceleration` is by default applied to all active nodes in the model if `[5 ...]` is not assigned.
2. `cload` stands for concentrated load, which is nodal force.
3. The true load magnitude is the product of reference magnitude and amplitude. This is similar to ABAQUS.
4. The multi-point displacement control algorithm [`MPDC`](../../Library/Solver/MPDC.md) is automatically enabled if a `displacement` or `groupdisplacement` is used.
5. Optionally, the displacement can be applied by using [`MPC`](../../Library/Constraint/MPC.md) constraint.
6. The multi-point displacement control algorithm [`MPDC`](../../Library/Solver/MPDC.md) is automatically enabled if a `supportdisplacement`, `supportvelocity` and/or `supportacceleration` are used.
7. 