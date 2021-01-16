# Guide

Try the following flow chart to find the best membrane element to use.

1. Is the geometry regular or irregular?
   * regular --- use quadrilateral elements
   * irregular --- use triangular elements

## Triangular Elements

1. Is drilling DoF required?
   * yes --- [Allman](Allman.md)
   * no --- go to Q2
2. Prefer speed or accuracy?
   * speed --- [CP3](CP3.md)
   * accuracy --- [CP6](CP6.md)

Noting that [CP3](CP3.md) element is very rigid and often requires a very dense mesh to get relatively accurate result, it is not recommended for most applications.

## Quadrilateral Elements

1. Is drilling DoF required?
   * yes --- go to Q2
   * no --- go to Q3
2. Is consistent material stiffness available?
   * yes --- go to Q6
   * no --- [GQ12](GQ12.md)
3. Used in elastoplastic applications?
   * yes --- normally mixed formulation is better, go to Q4, alternatively displacement based formulation, go to Q5
   * no --- go to Q5
4. Prefer speed or accuracy?
   * speed --- [PS](PS.md)
   * accuracy --- [QE2](QE2.md)
5. Prefer speed or accuracy?
   * speed --- [CP4I](CP4I.md) or second option [CP4](CP4.md)
   * accuracy --- [CP8R](CP8.md) or second option [CP8](CP8.md)
6. Need faster analysis?
   * yes --- [SGCMQ](GCMQ.md)
   * no --- [GCMQ](GCMQ.md)
