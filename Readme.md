# Parametric Curve Fitting

## Overview

This project estimates the unknown parameters of a given parametric
curve from a set of observed `(x, y)` points.

The unknown parameters are:

- `theta` — rotation angle
- `M` — exponential growth/decay parameter
- `X` — horizontal translation

The given parameter constraints are:

- `6 < t < 60`
- `0 < theta < 50 degrees`
- `-0.05 < M < 0.05`
- `0 < X < 100`

The main objective is to find the values of `theta`, `M`, and `X` that
provide the best fit to the given observations.

---

## Dataset

The input dataset is `xy_data.csv`.

It contains 1500 observations with two columns:

- `x`
- `y`

The data points are treated as unordered observations. The parameter
`t` is not directly provided in the dataset, so it needs to be inferred
during the parameter estimation process.

---

## Parametric Model

The given parametric curve is defined as:

$$
x(t)=t\cos(\theta)-e^{M|t|}\sin(0.3t)\sin(\theta)+X
$$

$$
y(t)=42+t\sin(\theta)+e^{M|t|}\sin(0.3t)\cos(\theta)
$$

where:

- `t` is the curve parameter
- `theta` is the rotation angle
- `M` controls the growth or decay of the oscillation
- `X` represents the horizontal translation

The objective is to estimate `theta`, `M`, and `X` from the observed
`x` and `y` coordinates.

---
## Approach

The observed data contains only `(x, y)` coordinates, while the
parameter `t` is unknown.

I first rewrite the equations by defining:

$$
A=e^{M|t|}\sin(0.3t)
$$

This gives:

$$
x-X=t\cos(\theta)-A\sin(\theta)
$$

$$
y-42=t\sin(\theta)+A\cos(\theta)
$$

Rotating the coordinates back gives:

$$
t=(x-X)\cos(\theta)+(y-42)\sin(\theta)
$$

and:

$$
A_{\text{observed}}
=-(x-X)\sin(\theta)+(y-42)\cos(\theta)
$$

For a candidate parameter set (`theta`, `M`, `X`), `t` can therefore be
inferred directly from every observed point.

The residual is:

$$
r=A_{\text{observed}}
-e^{M|t|}\sin(0.3t)
$$

The parameters are estimated by minimizing these residuals using
nonlinear least-squares optimization with the specified parameter
bounds.
