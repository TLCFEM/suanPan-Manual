# Viscosity01

Viscous Damper

The damping force is defined as a function of velocity (or strain rate, depends on what is the input).

$$
\sigma=\text{sign}(\dot\varepsilon)~\eta~|\dot\varepsilon|^\alpha.
$$

The damping coefficient is constant.

## Syntax

```
material Viscosity01 (1) (2) (3)
# (1) int, unique tag
# (2) double, alpha
# (3) double, damping coefficient
```
