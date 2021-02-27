# BatheTwoStep

In the original paper ([10.1016/j.compstruc.2006.09.004](https://doi.org/10.1016/j.compstruc.2006.09.004)), a single time step is divided into two sub-steps. This creates complexity in terms of implementation. Instead, a leap-frog style algorithm is implemented: odd steps perform trapezoidal rule and even steps perform backward Euler rule. Thus the real time increment chosen should be half to the desired one.

The algorithm is known to be able to conserve energy and momentum.

## Syntax

```
integrator BatheTwoStep (1)
# (1) int, unique integrator tag
```

## Trapezoidal Rule

For trapezoidal rule,

$$
\begin{align}
v_{n+1}&=v_n+\dfrac{\Delta{}t}{2}\left(a_n+a_{n+1}\right),\\[4mm]
u_{n+1}&=u_n+\dfrac{\Delta{}t}{2}\left(v_n+v_{n+1}\right).
\end{align}
$$

Then,

$$
\begin{align}
u_{n+1}&=u_n+\dfrac{\Delta{}t}{2}\left(v_n+v_n+\dfrac{\Delta{}t}{2}\left(a_n+a_{n+1}\right)\right),\\[4mm]
\Delta{}u&=\Delta{}tv_n+\dfrac{\Delta{}t^2}{4}a_n+\dfrac{\Delta{}t^2}{4}a_{n+1}.
\end{align}
$$

One could obtain

$$
\begin{align}
a_{n+1}&=\dfrac{4}{\Delta{}t^2}\Delta{}u-\dfrac{4}{\Delta{}t}v_n-a_n,\qquad
\Delta{}a=\dfrac{4}{\Delta{}t^2}\Delta{}u-\dfrac{4}{\Delta{}t}v_n-2a_n,\\[4mm]
v_{n+1}&=\dfrac{2}{\Delta{}t}\Delta{}u-v_n,\qquad
\Delta{}v=\dfrac{2}{\Delta{}t}\Delta{}u-2v_n.
\end{align}
$$

The effective stiffness is then

$$
\bar{K}=K+\dfrac{2}{\Delta{}t}C+\dfrac{4}{\Delta{}t^2}M.
$$

## Euler Rule

The second step is computed by using the backward Euler method. Thus

$$
\begin{align}
v_{n+2}&=\dfrac{1}{2\Delta{}t}u_n-\dfrac{2}{\Delta{}t}u_{n+1}+\dfrac{3}{2\Delta{}t}u_{n+2},\\[4mm]
a_{n+2}&=\dfrac{1}{2\Delta{}t}v_n-\dfrac{2}{\Delta{}t}v_{n+1}+\dfrac{3}{2\Delta{}t}v_{n+2}.
\end{align}
$$

Hence,

$$
\begin{align}
v_{n+2}=\dfrac{1}{2\Delta{}t}u_n-\dfrac{1}{2\Delta{}t}u_{n+1}+\dfrac{3}{2\Delta{}t}\Delta{}u,\\[4mm]
a_{n+2}=\dfrac{1}{2\Delta{}t}v_n-\dfrac{2}{\Delta{}t}v_{n+1}+\dfrac{3}{2\Delta{}t}v_{n+2}.
\end{align}
$$

The effective stiffness is then

$$
\bar{K}=K+\dfrac{3}{2\Delta{}t}C+\dfrac{9}{4\Delta{}t^2}M.
$$