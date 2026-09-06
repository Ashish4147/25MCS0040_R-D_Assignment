# Parametric Curve Fitting

## Overview

This project estimates the unknown parameters of a given parametric
curve from a set of observed (x, y) points.

The unknown parameters are:

- 'theta' — rotation angle
- 'M' — exponential growth/decay parameter
- 'X' — horizontal translation

The provided constraints are:

- '6 < t < 60'
- '0 < theta < 50 degrees'
- '-0.05 < M < 0.05'
- '0 < X < 100'


## Parametric Model

The curve is defined by:

x(t)=t\cos(\theta)-e^{M|t|}\sin(0.3t)\sin(\theta)+X


y(t)=42+t\sin(\theta)+e^{M|t|}\sin(0.3t)\cos(\theta)


The objective is to estimate 'theta', 'M', and 'X' from the observed
'x' and 'y' coordinates.

---

## Approach

The observed data contains only `(x, y)` coordinates, while the
parameter 't' is unknown.

I first rewrite the equations by defining:

A=e^{M|t|}\sin(0.3t)

This gives:

x-X=t\cos(\theta)-A\sin(\theta)


y-42=t\sin(\theta)+A\cos(\theta)

Rotating the coordinates back gives:

t=(x-X)\cos(\theta)+(y-42)\sin(\theta)

and

A_{\text{observed}}
=-(x-X)\sin(\theta)+(y-42)\cos(\theta)

For a candidate parameter set '(theta, M, X)', 't' can therefore be
inferred directly from every observed point.

The residual is:

r=A_{\text{observed}}
-e^{M|t|}\sin(0.3t)

The parameters are estimated by minimizing these residuals using
nonlinear least-squares optimization with the specified parameter
bounds.


## Optimization

The implementation uses SciPy's 'least_squares' optimizer.

The optimization variables are:

```text
theta, M, X