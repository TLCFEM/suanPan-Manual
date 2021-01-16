# LeeNewmark

Newmark Algorithm With Lee Damping Model

Please check the references for theory.

1. [10.1016/j.jsv.2020.115312](https://doi.org/10.1016/j.jsv.2020.115312)
2. [10.1016/j.engstruct.2020.110178](https://doi.org/10.1016/j.engstruct.2020.110178)

**For the moment, MPC cannot be considered in all global damping models.**

## Syntax

```
integrator LeeNewmark (1) (2) (3) ((4) (5) ...)
# (1) int, unique integrator tag
# (2) double, alpha
# (3) double, beta
# (4) double, damping ratio \zeta_p at the peak of each mode
# (5) double, circular frequency \omega_p at the peak of each mode
```

## Remarks

The `LeeNewmark` integrator uses a standard Newmark algorithm and the damping model proposed by Lee (2020).

The static condensation procedure is reversed so that the damping matrix $$\mathbf{C}$$ is a sparse matrix of a different size.

Currently, the modified equation of motion (of larger size) is stored as a sparse matrix and the single threaded `SuperLU` solver is used to solve sparse systems. The matrix storage flags (`sparse_mat`, `band_mat`, `symm_mat`) still have effect on how original matrices are stored but do not affect solving stage. Depending on different storage schemes (dense/sparse), the assembly of final effective stiffness may have different efficiency. For large systems, it may potentially be faster if original matrices adopt sparse scheme as well. To do so, one can use the following command.

```
set sparse_mat true
```

## Example

If one wants to assign $$3\%$$ damping on three basic functions with peaks located at $$\omega_1=1$$, $$\omega_2=10$$ and $$\omega_3=100$$, one can use

```
integrator LeeNewmark 1 .25 .5 .03 1 .03 10 .03 100
```

With the above command, we use $$\alpha=0.25$$ and $$\beta=0.5$$ in Newmark method. Note the final overall response will be something greater than $$3\%$$ at those three frequencies since the contributions of three functions will be summed. Users need to manually compute $$\zeta_p$$ and $$\omega_p$$ to obtain desired curve.
