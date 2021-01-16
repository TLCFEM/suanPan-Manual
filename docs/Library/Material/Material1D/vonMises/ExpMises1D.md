# ExpMises1D

## Theory

The backbone function is defined as

$$
\sigma=a-ae^{-b\varepsilon_p}+cE\varepsilon_p,
$$

where $$a$$ and $$b$$ are two constants that control the shape of the backbone. The initial stiffness $$\sigma'|_{\varepsilon_p=0}=ab$$.

## Syntax

```
material ExpMises1D (1) (2) (3) (4) (5) (6) [7]
# (1) int, unique tag
# (2) double, elastic modulus
# (3) double, yield stress
# (4) double, a
# (5) double, b
# (6) double, c
# [7] double, density, default: 0
```
