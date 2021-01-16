# RayleighNewmark

Newmark Algorithm With Rayleigh Damping Model

The `RayleighNewmark` integrator is simply a wrapper of the `Newmark` integrator with Rayleigh model implemented on global level.

**For the moment, MPC cannot be considered in all global damping models.**

```
integrator RayleighNewmark (1) (2) (3) (4) [5] [6]
# (1) int, unique tag
# (2) double, alpha
# (3) double, beta
# (4) double, theta
# [5] double, alpha in Newmark method, default: 0.25
# [6] double, beta in Newmark method, default: 0.5
```

## Theory

By default, the Rayleigh type damping model is used. No matter what other damping modifiers are defined, this integrator always set the element damping matrix to be

$$
C=\alpha{}M+\beta{}K_C+\theta{}K_I
$$

by the `Rayleigh` modifier in addition to the existing damping matrix defined in the element. In the above formulation, $$K_C$$ is the current converged stiffness, $$K_I$$ is the initial stiffness of the element. The trial stiffness $$K_T$$ is **not** accounted for in this formulation due to that, $$K_T$$ is updated in each iteration and is a function of trial displacement, if $$K_T$$ is involved, the damping force is then a function of trial displacement. The quadratic convergence rate is lost then.
