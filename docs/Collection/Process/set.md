# set

The `set` command is used to set some properties of the analysis.

## Substepping Control

To use a fixed substep size, users can define

```
set fixed_step_size true
```

Otherwise, the algorithm will automatically substep the current step if convergence is not met. The time step control strategy cannot be customized.

To define the initial step size, users can use

```
set ini_step_size (1)
# (1) double, substep size
```

To define the maximum/minimum step size, users can use

```
set max_step_size (1)
set min_step_size (1)
# (1) double, target time step size
```

## Matrix Storage Scheme

### Asymmetric Banded

By default, an **asymmetric banded** matrix storage scheme is used. For 1D analysis, the global stiffness matrix is always symmetric. However, when it comes to 2D and 3D analyses, the global stiffness may be symmetric in terms of its layout, but the may not have the same value on the symmetric entries. In math language, if $$K(i,j)\neq0$$ then $$K(j,i)\neq0$$, but $$K(i,j)\neq{}K(j,i)$$. Hence an asymmetric banded storage is the safest. The `_gbsv()` LAPACK subroutine is used for matrix solving.

### Symmetric Banded

To use a symmetric banded storage, the following commands can be used.

```
set symm_mat true
set band_mat true
```

The above commands enable a symmetric scheme. It shall be noted that the symmetric scheme can save almost $$50\%$$ of the memory used in the asymmetric scheme. The `_pbsv()` LAPACK subroutine is used. This subroutine can only deal with **symmetric positive definite band matrix**. For some problems, in which the matrix is not necessarily positive definite, for example the buckling problems, this subroutine fails. **Before enabling the symmetric banded storage, the analyst must check if the matrix is SPD.**

Use the following command to enable `Spike` solver.

```
set system_solver Spike
```

### Full Storage

If the problem scale is small, it does not hurt if a full storage scheme is used.

```
set symm_mat false
set band_mat false
```

Use the following command to enable `CUDA` sparse solver for `CUDA` enabled version.

```
set system_solver CUDA
```

### Full Packed Storage

If the matrix is symmetric, a so called pack format can be used to store the matrix. Essentially, only the upper or the lower triangle of the matrix is stored. The spatial cost is half of that of the full storage, but the solving speed is no better. The `_spsv()` subroutine is used for matrix solving.

```
set symm_mat true
set band_mat false
```

### Sparse Storage

The sparse matrix is also supported. The following command will ignore any previous symmetric or banded settings and uses sparse solver to solve the matrix.

```
set sparse_mat true
```

Use the following command to switch among solvers.

```
set system_solver SuperLU
set system_solver MUMPS
set system_solver CUDA
```

## System Solver

There are several different solvers implemented. It is possible to switch from one to another.

```
set system_solver (1)
# (1) string, system solver name 
```

For sparse storage, three solvers are available as presented above: `CUDA`, `SuperLU` and `MUMPS`. The solving speed of both `SuperLU` and `MUMPS` is slow compared to that of dense solvers. Multithreaded versions do not always result in faster solving. Factors such as problem size, computation platform, overhead, etc. all may slow down the procedure.

The following command can be used to control if to use mixed precision refinement. This command has no effect if the target matrix storage scheme has no mixed precision implementation.

```
set precision (1)
# (1) string, "single" or "double"
```

## Parallel Matrix Assembling

For dense matrix storage schemes, the global matrix is stored in a consecutive chunk of memory. Assembling global matrix needs to fill in the corresponding memory location with potentially several values contributed by different elements. Often in-place atomic summation is involved. To assign each memory location a mutex lock is not cost efficient. Instead, a $$k$$-coloring concept can be adopted to divide the whole model into several groups. Each group contains elements that do not shared common nodes with others in the same group. By such, no atomic operations are required. Elements in the same group can be updated simultaneously so the matrix assembling is lock free.

By default, the coloring algorithm is enabled. To disable it, users can use the following command.

```
set color_model false
```

As a NP hard problem, there is no optimal algorithm to find the minimum chromatic number. The Welsh-Powell algorithm is implemented in `suanPan`.

Also, depending on the problem setup, such a coloring may or may not help to improve the performance. If there are not a large number of matrix assembling, the time saved may not be significant. Thus for problems of small sizes, users may consider to disable coloring.

This option has no effect if a sparse storage is used.

## Penalty Number

For some constraints and loads that are implemented by using the penalty method, the default penalty number can be overridden.

```
set constraint_multiplier (1)
set load_multiplier (1)
# (1) double, new penalty number
```

This command does not overwrite user defined penalty number if the specific constraint or load takes the penalty number as input arguments.
