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
