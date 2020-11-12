## Step size/ convergence rate/ complexity

| Algorithms       | Usage                        | Convex                                                       | Convex+Smooth                                                | Strongly convex                                       | Strongly convex+Smooth                                       |
| ---------------- | ---------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ----------------------------------------------------- | ------------------------------------------------------------ |
| Gradient Descent | Differentiable               | Can diverge                                                  | $\frac{1}{M}$ or  $\frac{\beta}{M}$;$\frac{1}{T}$; $\frac{1}{\varepsilon}$ | Can diverge                                           | $\frac{1}{M}$ ;$(1-\frac{m}{M})^T$; $\log\frac{1}{\varepsilon}$ |
| Subgradient      | Non-differentiable           | $\frac{1}{\sqrt{t}}$ ;$\frac{1}{\sqrt{T}}$; $\frac{1}{\varepsilon^2}$ | $\frac{1}{\sqrt{t}}$ ;$\frac{1}{\sqrt{T}}$; $\frac{1}{\varepsilon^2}$ | $\frac{1}{t}$ ;$\frac{1}{T}$; $\frac{1}{\varepsilon}$ | $\frac{1}{t}$ ;$\frac{1}{T}$; $\frac{1}{\varepsilon}$        |
| Frank Wolfe      | Projection free, constrained | $\frac{1}{t}$ ;$\frac{1}{T}$; $\frac{1}{\varepsilon}$        | $\frac{1}{t}$ ;$\frac{1}{T}$; $\frac{1}{\varepsilon}$        | $\frac{1}{t}$ ;$\frac{1}{T}$; $\frac{1}{\varepsilon}$ | $\frac{1}{t}$ ;$\frac{1}{T}$; $\frac{1}{\varepsilon}$        |
|                  |                              |                                                              |                                                              |                                                       |                                                              |
|                  |                              |                                                              |                                                              |                                                       |                                                              |
|                  |                              |                                                              |                                                              |                                                       |                                                              |
|                  |                              |                                                              |                                                              |                                                       |                                                              |
|                  |                              |                                                              |                                                              |                                                       |                                                              |
|                  |                              |                                                              |                                                              |                                                       |                                                              |

Proximal gradient descent: same as GD for decomposable objective function.

Projected gradient descent: same as GD but projection step may be slow

Frank Wolfe: projection-free. $O(1/k)$ sublinear

