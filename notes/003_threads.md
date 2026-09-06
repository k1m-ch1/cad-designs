# Helix shape

So parametrically, it's actually kinda easy to define the helix shape.

We know that in 2d, the circle can be parametrically defined:

$$
R\begin{bmatrix}
\cos(t)\\
\sin(t)
\end{bmatrix}
$$

The helix shape can be achieved simply linearly translating the parameter $t$ in the z-axis.

$$
\begin{bmatrix}
R\cos(t)\\
R\sin(t)\\
t
\end{bmatrix}
$$

Of course, we get a helix shape, although, the pitch is in this case, $2\pi$.

Essentially, if we have parameter:

$$
\begin{bmatrix}
R\cos(\frac{2\pi}{T} t)\\
R\sin(\frac{2\pi}{T} t)\\
\alpha t
\end{bmatrix}
$$

Then, we essentially have period T, therefore, after T, z would have moved $\alpha T$ and if we want the pitch $p$ to be our parameter:

$$
\alpha T = p \implies \alpha = \frac{p}{T}
$$

So, the helix shape, given a pitch $p$:

$$
\begin{bmatrix}
R \cos(\frac{2\pi}{T} t)\\
R \sin(\frac{2\pi}{T} t)\\
\frac{p}{T} t
\end{bmatrix}
$$

# Designing screws

So, screws follow a helix shape, however, we're not dealing with a 1d path, rather, it's something 3d. We can just think of it however, as having a set of helixes that together, form a surface or something like that.

But essentially, if we just provide the pitch, we need to then provide the cross-sectional profile that we want the helix to form.

Normally, we use some sort of equilateral triangle, or even if it isn't an equilateral, we use at least an isosceles triangle as a base. Suppose we have an isosceles triangle as such:

```
  /|
 / |
/  |
\  |
 \ |
  \|
```

If the base is of length $b$, and of height $h$, then theoretically, if we want the male screw to fit to the female screw, it has to be the case that the pitch $p \geq b$. Moreover, the male's base cylindrical radius must be at least $h$ smaller than the outer female screw.
