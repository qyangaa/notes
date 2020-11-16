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

**Proximal gradient descent:** same as GD for decomposable objective function.

**Projected gradient descent**: same as GD but projection step may be slow

**Coordinate descent:** similar to GD for function decomposable to smooth + separable non-smooth parts.

**Frank Wolfe**: projection-free. $O(1/k)$ sublinear



## Definition of convergence rate

[ref](https://www.stat.cmu.edu/~ryantibs/convexopt-F13/scribes/lec9.pdf)



1. Linear convergence rate $(\delta<1)$ :
$$
\begin{array}{l}
\text { Example: } s_{i}=c q^{i}, 0<q<1, \bar{s}=0 \\
\lim _{i \rightarrow \infty} \frac{\left|s_{i+1}-\bar{s}\right|}{\left|s_{i}-\bar{s}\right|}=\lim _{i \rightarrow \infty} \frac{c q^{i+1}}{c q^{i}}=q<1
\end{array}
$$
2. Superlinear convergence rate $(\delta=0)$ :
$$
\text { Example: } s_{i}=\frac{c}{i !}, \bar{s}=0
$$
$$
\lim _{i \rightarrow \infty} \frac{\left|s_{i+1}-\bar{s}\right|}{\left|s_{i}-\bar{s}\right|}=\lim _{i \rightarrow \infty} \frac{c i !}{c(i+1) !}=\lim _{i \rightarrow \infty} \frac{1}{i+1}=0
$$
3. Sublinear convergence rate $(\delta=1)$ :
$$
\begin{array}{l}
\text { Example: } s_{i}=\frac{c}{i^{a}}, a>0, \bar{s}=0 \\
\lim _{i \rightarrow \infty} \frac{\left|s_{i+1}-\bar{s}\right|}{\left|s_{i}-\bar{s}\right|}=\lim _{i \rightarrow \infty} \frac{c i^{a}}{c(i+1)^{a}}=\lim _{i \rightarrow \infty}\left(\frac{i}{i+1}\right)^{a}=1
\end{array}
$$
4. Quadratic convergence rate $\left(\lim _{i \rightarrow \infty} \frac{\left|s_{i+1}-\bar{s}\right|}{\left|s_{i}-\bar{s}\right|^{2}}<\infty\right)$
$$
\text { Example: } s_{i}=q^{2^{i}}, 0<q<1, \bar{s}=0
$$
$$
\lim _{i \rightarrow \infty} \frac{\left|s_{i+1}-\bar{s}\right|}{\left|s_{i}-\bar{s}\right|^{2}}=\lim _{i \rightarrow \infty} \frac{q^{2^{i+1}}}{q^{2^{i}}}=\lim _{i \rightarrow \infty} q^{2^{i}}=0
$$