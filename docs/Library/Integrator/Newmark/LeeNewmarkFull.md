# LeeNewmarkFull

Newmark Algorithm With Lee Damping Model (Full Modes)

Please check the references for theory.

1. [10.1016/j.jsv.2020.115312](https://doi.org/10.1016/j.jsv.2020.115312)
2. [10.1016/j.engstruct.2020.110178](https://doi.org/10.1016/j.engstruct.2020.110178)
3. [10.1016/j.compstruc.2020.106423](https://doi.org/10.1016/j.compstruc.2020.106423)

**For the moment, MPC cannot be considered in all global damping models.**

## Syntax

```
integrator LeeNewmarkFull (1) (2) (3) ((4) (5) (6) [7...] ...)
# (1) int, unique integrator tag
# (2) double, alpha in Newmark method
# (3) double, beta in Newmark method
# (4) string, type identifier
# (5) double, \zeta_p
# (6) double, \omega_p
# (7...) double/int, parameters associated with the mode 
```

## Remarks

1. The `LeeNewmark` integrator uses a standard Newmark algorithm and the damping model proposed by Lee (2020). Several different types of basic functions are defined to control the bandwidth.

2. The static condensation procedure is reversed so that the damping matrix $$\mathbf{C}$$ is a sparse matrix of a different size.

3. Currently, the modified equation of motion (of larger size) is stored as a sparse matrix and the single threaded `SuperLU` solver is used to solve sparse systems. Multithreaded sparse solver does not always result in faster solving, depending on the problem size, overhead, etc.

4. The final global matrix is expected to be considerable huge compared to the original matrix. Thus the sparse storage scheme is also used for assembling original stiffness and mass matrices by default. Please **always** add the following command in the corresponding step.

   ```
   set sparse_mat true
   ```

5. The following type identifiers are available:

   1. `-type0` --- zeroth order model, need no additional parameter, viz., `[7]` can be empty
   2. `-type1` --- type 1 model, need one integer parameter, viz., `[7]` ($$n_p$$) is a non-negative integer
   3. `-type2` --- type 2 model, need two integer parameters, viz., `[7]` ($$n_{pl}$$) is a non-negative integer (**zero allowed**) and `[8]` ($$n_{pr}$$) is a positive integer (**excluding zero**)
   4. `-type3` --- type 3 model, need a double parameter, viz., [7] ($$\gamma$$) is a float number that is greater than $$\gamma>-1$$

6. Multiple types can be mixed in any preferred orders. See examples below.

## Examples

The following commands define $$5\%$$ damping on $$\omega=1$$ via different types with different parameters.

```
integrator LeeNewmarkFull 1 .25 .5 -type0 .05 1
integrator LeeNewmarkFull 1 .25 .5 -type1 .05 1 2
integrator LeeNewmarkFull 1 .25 .5 -type2 .05 1 2 1
integrator LeeNewmarkFull 1 .25 .5 -type2 .05 1 0 3
integrator LeeNewmarkFull 1 .25 .5 -type2 .05 1 2 3
integrator LeeNewmarkFull 1 .25 .5 -type3 .05 1 -.25
integrator LeeNewmarkFull 1 .25 .5 -type3 .05 1 .5
integrator LeeNewmarkFull 1 .25 .5 -type3 .01 1 .5 -type2 .01 1 2 3 -type2 .01 1 2 1 -type0 .01 1 -type1 .01 1 2
```

With the above commands, we use $$\alpha=0.25$$ and $$\beta=0.5$$ in Newmark method. Users need to manually compute $$\zeta_p$$ and $$\omega_p$$ to obtain desired curve.
