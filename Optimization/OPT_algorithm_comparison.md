## Step size/ convergence rate/ complexity

| Algorithms       | Best<br />Convergence | Usage                        | Convex                                                       | Convex+Smooth                                                | Strongly convex                                       | Strongly convex+Smooth                                       |
| ---------------- | --------------------- | ---------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ----------------------------------------------------- | ------------------------------------------------------------ |
| Gradient Descent | Linear                | Differentiable               | Can diverge                                                  | $\frac{1}{M}$ or  $\frac{\beta}{M}$;$\frac{1}{T}$; $\frac{1}{\varepsilon}$ | Can diverge                                           | $\frac{1}{M}$ ;$(1-\frac{m}{M})^T$; $\log\frac{1}{\varepsilon}$ |
| Subgradient      | Sublinear             | Non-differentiable           | $\frac{1}{\sqrt{t}}$ ;$\frac{1}{\sqrt{T}}$; $\frac{1}{\varepsilon^2}$ | $\frac{1}{\sqrt{t}}$ ;$\frac{1}{\sqrt{T}}$; $\frac{1}{\varepsilon^2}$ | $\frac{1}{t}$ ;$\frac{1}{T}$; $\frac{1}{\varepsilon}$ | $\frac{1}{t}$ ;$\frac{1}{T}$; $\frac{1}{\varepsilon}$        |
| Frank Wolfe      | Sublinear             | Projection free, constrained | $\frac{1}{t}$ ;$\frac{1}{T}$; $\frac{1}{\varepsilon}$        | $\frac{1}{t}$ ;$\frac{1}{T}$; $\frac{1}{\varepsilon}$        | $\frac{1}{t}$ ;$\frac{1}{T}$; $\frac{1}{\varepsilon}$ | $\frac{1}{t}$ ;$\frac{1}{T}$; $\frac{1}{\varepsilon}$        |
| Newton           | Quadratic             |                              |                                                              |                                                              |                                                       | $\frac{f\left(x_{0}\right)-f^{*}}{\gamma}+\log \log \left(\frac{\varepsilon_{0}}{\varepsilon}\right)$ |
| Quasi-Newton     | Superlinear           |                              |                                                              |                                                              |                                                       |                                                              |
| Barrier Method   | Linear                |                              |                                                              |                                                              |                                                       | $\left[\frac{\log \left(m /\left(\epsilon t^{(0)}\right)\right)}{\log \mu}\right\rceil$ * Newton |
| GD (not diverge) | Linear                | $O(nd)$ per iteration        | $O(\frac{1}{\sqrt{T}})$                                      | $O(\frac{1}{T})$                                             | $O(c^T)$                                              | $\log\frac{1}{\varepsilon}$                                  |
| SGD              | Sublinear             | $O(d)$ per iteration         | $O(\frac{1}{\sqrt{T}})$                                      | $O(\frac{1}{\sqrt{T}})$                                      | $O(\frac{1}{T})$$                                     | $\frac{1}{t}$ ;$\frac{1}{T}$; $\frac{1}{\varepsilon}$        |
| Minibatch SGD    | Sublinear             | $O(bd)$ per iteration        | $O(\frac{1}{\sqrt{T}})$                                      | $O\left(\frac{1}{\sqrt{b T}}+\frac{1}{T}\right)$             | $O(\frac{1}{T})$$                                     | $\frac{1}{t}$ ;$\frac{1}{T}$; $\frac{1}{\varepsilon}$        |
| SAG/ SAGA        | Linear                |                              | $O(\frac{1}{\sqrt{T}})$                                      | $O(\frac{1}{T})$                                             | $O(c^T)$                                              | $\log\frac{1}{\varepsilon}$                                  |
|                  |                       |                              |                                                              |                                                              |                                                       |                                                              |
|                  |                       |                              |                                                              |                                                              |                                                       |                                                              |

- **Proximal gradient descent:** same as GD for decomposable objective function.

- **Projected gradient descent**: same as GD but projection step may be slow
- **Coordinate descent:** similar to GD for function decomposable to smooth + separable non-smooth parts.

- **Frank Wolfe**: projection-free. $O(1/k)$ sublinear
- **Newton**:  Quadratic convergence $\frac{f\left(x_{0}\right)-f^{*}}{\gamma}+\log \log \left(\frac{\varepsilon_{0}}{\varepsilon}\right)$
  - Strongly convex smooth functions:
    - Damped phase: $\eta$ from backtrack, $\left(\|\nabla f(x)\|_{2} \geq \alpha\right)$, at most $\left(f\left(x^{(0)}\right)-p^{\star}\right) / \gamma$ iterations
    - pure phase: $\eta = 1$, $\left(\|\nabla f(x)\|_{2}<\alpha\right)$, $f(x_k)-f^* \le \frac{2 m^{3}}{L^{2}}\left(\frac{1}{2}\right)^{k-k_{0}+1}$
  - Self concordant objective function: quadratic convergence without unknown constants: $c(\alpha, \beta)\left(f\left(x_{0}\right)-f *\right)+\log \log \frac{1}{\varepsilon}$

+ __Quasi Newton:__ Super linear convergence $\lim _{t \rightarrow \infty} \frac{\left\|\boldsymbol{x}^{t+1}-\boldsymbol{x}^{*}\right\|_{2}}{\left\|\boldsymbol{x}^{t}-\boldsymbol{x}^{*}\right\|_{2}}=0$
+ __Barrier method__: # Newton steps * # outer iterations
  + #outer iterations = $\left[\frac{\log \left(m /\left(\epsilon t^{(0)}\right)\right)}{\log \mu}\right\rceil$,Linear
+ __Accelerated Gradient descent__: add momentum to reach lower bound of convergence speed
+ **Variance reduction**: modify SGD, use averaged updated and not updated coordinate gradient values for $x$ update

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