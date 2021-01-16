# Sequential

Container of Material Models In Sequence

## Syntax

```
material Sequential (1) (2 ...)
# (1) int, unique material tag
# (2 ...) int, material tags of 1D models need to be contained
```

## Caveat

Please do not include any viscosity related models into the wrapper. This container is only designed for rate-independent hysteric models.
