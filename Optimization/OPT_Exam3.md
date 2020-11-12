# Optimization Algorithms

## Subgradient Basics

$$
f(y)-f(x) \ge g^T(y-x)\\
\partial f(x) = \text{set}(g)
$$

### Basic rules

- scaling: $\partial(\alpha f)=\alpha \partial f($ for $\alpha>0)$
- summation: $\partial\left(f_{1}+f_{2}\right)=\partial f_{1}+\partial f_{2}$
- affine transformation: $\quad$ if $h(\boldsymbol{x})=f(\boldsymbol{A} \boldsymbol{x}+\boldsymbol{b}),$ then

$$
\partial h(\boldsymbol{x})=\boldsymbol{A}^{\top} \partial f(\boldsymbol{A} \boldsymbol{x}+\boldsymbol{b})
$$

- chain rule: suppose $f$ is convex, and $g$ is differentiable, nondecreasing, and convex. Let $h=g \circ f,$ then
$$
\partial h(\boldsymbol{x})=g^{\prime}(f(\boldsymbol{x})) \partial f(\boldsymbol{x})
$$
- composition: suppose $f(\boldsymbol{x})=h\left(f_{1}(\boldsymbol{x}), \cdots, f_{n}(\boldsymbol{x})\right),$ where $f_{i}$ 's
are convex, and $h$ is differentiable, nondecreasing, and convex. Let $\boldsymbol{q}=\left.\nabla h(\boldsymbol{y})\right|_{\boldsymbol{y}=\left[f_{1}(\boldsymbol{x}), \cdots, f_{n}(\boldsymbol{x})\right]},$ and $\boldsymbol{g}_{i} \in \partial f_{i}(\boldsymbol{x}) .$ Then

$$
q_{1} \boldsymbol{g}_{1}+\cdots+q_{n} \boldsymbol{g}_{n} \in \partial f(\boldsymbol{x})
$$

- point-wise maximum: if $f(x)=\max _{1 \leq i \leq k} f_{i}(x),$ then
$$
\partial f(\boldsymbol{x})=\underbrace{\operatorname{conv}\left\{\bigcup\left\{\partial f_{i}(\boldsymbol{x}) \mid f_{i}(\boldsymbol{x})=f(\boldsymbol{x})\right\}\right\}}_{\text {convex hull of subdifferentials of all active functions }}
$$
+ point-wise supremum: $\quad$ if $f(x)=\sup _{\alpha \in \mathcal{F}} f_{\alpha}(x),$ then

$$
\partial f(\boldsymbol{x})=\text { closure }\left(\operatorname{conv}\left\{\bigcup\left\{\partial f_{\alpha}(\boldsymbol{x}) \mid f_{\alpha}(\boldsymbol{x})=f(\boldsymbol{x})\right\}\right\}\right)
$$



## Subgradient Method

### Application

For **non-differentiable** functions

Example: GD oscillates in:

+ eg. $f(x) = |x|$, $x_{+}=x-\eta \operatorname{sgn}(x)$, oscillation for any step size $\eta$

### Algorithm

$$
x_{t+1}=x_{t}-\eta_{t} g_{t}\\
\text{find }x_{\text {best }}: f\left(x_{\text {best }}^{(t)}\right)=\min _{s \leq t} f\left(x_{s}\right)
$$



### Properties

+ Not a descent method: $(-g)$ might not be the descent direction, thus selecting best past point

#### Convergence Rate

**For fixed $\eta$: (doesn't work)**
$$
\lim _{t \rightarrow \infty} f\left(x_{\text {best}}^{(t)}\right) \leq f^{*}+\frac{\eta G^{2}}{2}
$$


+ $G$ is the Lipschitz constant of $f$ :$\|f(x)-f(y) \| \leq G\| x-y \|$
+ Then we have to make $\eta \rightarrow 0$ to converge to $f^*$, which won't work because $\Sigma_{t} \eta_{t} \rightarrow \infty$ is also required for enough update distances.

**For variable $\eta_t$:**
$$
f\left(x_{\text {best }}^{(t)}\right)-f^{*} \leq \frac{R^{2}+G^{2} \sum_{s \leq t} \eta_{s}^{2}}{2 \sum_{s \leq t} \eta_{s}}=O\left(\frac{R^{2}+G^{2}}{\sqrt{t}}\right) \text{ if } \eta_{t}=\frac{1}{\sqrt{t}}
$$

+ $R=\left\|x_{0}-x^{*}\right\|$ and note that $\left\|x_{t}-x^{*}\right\|^{2} \geq 0,\left\|g_{s}\right\|^{2} \leq G^{2}$
+ Corresponds to $O\left(\frac{1}{\varepsilon^{2}}\right)$ convergence rate

### Comparing Gradient and subgradient algorithms

+ Metric: number of steps for $f(x)-f(x^*)$ to reach within some $\epsilon$
+ Convexity
  + convex: local optimal -> global optimal, $f$ can be globally approximated from below by linear function
  + smooth + convex: 
    + quadratically upper bounded -> self tuning 
    + no kink at optimal
    + $\nabla f(x) \rightarrow 0 \text{ as } x\rightarrow x^*$
  + strongly convex: 
    + quadratic lower bounded -> ($\nabla f(x)$) big steps when far away from optimal
    + Not looking at points close to optimal
+ Convergence rate 
  + $f$ convex: subgradient method $O\left(\frac{1}{\varepsilon^{2}}\right)$ steps, each step updates $O\left(\frac{1}{\sqrt T}\right)$
  + $f$ convex and smooth (differentiable everywhere): GD $O\left(\frac{1}{\varepsilon}\right)$ steps
  + $f$ strongly convex and not necessarily smooth: subgradient method $O\left(\frac{1}{\varepsilon}\right)$ steps
  + $f$ strongly convex and smooth: GD method $O\left(\log \frac{1}{\varepsilon}\right)$ steps

## Proximal Gradient Descent

### Application

For **Decomposable** objective functions:
$$
\min _{x} g(x)+h(x)
$$

+ $g$ is convex and smooth
+ $h$ is convex not smooth, with easy-to-calculate $\text{prox}(h)$

### Algorithm

**Prox operator**: $\mathbb{R}^n \rightarrow \mathbb{R}^n$, not a function but a mapping

$$
\begin{array}{c}
\operatorname{prox}_{\eta}(x)=\arg \min _{z}\left\{\frac{1}{2 \eta}\|x-z\|^{2}+h(z)\right\} \\
x_{+}=\operatorname{prox}_{\eta}(x-\eta \nabla g(x))
\end{array}
$$

### Properties

+ Useful when $prox$ operator is easy to calculate

+ $$
  \operatorname{prox}_{g}(\mathbf{x})=\operatorname{argmin}_{\mathbf{u} \in \mathbb{E}}\left\{\delta_{C}(\mathbf{u})+\frac{1}{2}\|\mathbf{u}-\mathbf{x}\|^{2}\right\}=\operatorname{argmin}_{\mathbf{u} \in C}\|\mathbf{u}-\mathbf{x}\|^{2}=P_{C}(\mathbf{x})\\
  \delta: \text{indicator function;} \quad P_{C}:\text{projection function}
  $$

#### Convergence Rate

+ with $\eta<\frac{1}{M}$ ($M$ is for $g(x)$ and $prox(h(x))$ is easy-to-calculate): $f\left(x_{t}\right)-f * \leq \frac{\left\|x_{0}-x^{*}\right\|^{2}}{2 \eta t}$, i.e. $O\left(\frac{1}{\varepsilon}\right)$, same as gradient descent

### Examples

|                            | Objective function                                           | argmin{} |Prox|
| -------------------------- | ------------------------------------------------------------ | -------- | ---- |
| Projected Gradient Descent | $\min _{x \in C} g(x) \Longleftrightarrow \min _{x \in \mathbb{R}^{n}} g(x)+I_{C}(x)$ |    $\frac{1}{2 t}\|x-z\|_{2}^{2}+I_{C}(z)= \|x-z\|_{2}^{2}$ | $P_c$ |
| Lasso | $\min _{\beta}\|y-X \beta\|^{2}+\lambda\|\beta\|_{1}$ | $\left\{\frac{1}{2 \eta}\|\beta-z\|^{2}+\lambda\|z\|_{1}\right\}$      | $\left\{\begin{array}{ll}\beta_{i}-\eta \lambda & \text { if } \beta_{i}>\eta \lambda \\\beta_{i}+\eta \lambda & \text { if } \beta_{i}<-\eta \lambda \\0 & \text { else }\end{array}\right.$|
| One-norm | $h(x)=\|x\|_{1}=\sum\left|x_{i}\right|$     |          |  soft thresholding operator: $z_{i}=\left\{\begin{array}{ll}x_{i}-1 & x_{i}>\eta \\0 & -\eta \leqslant x_{i} \leqslant \eta \\x_{i}+1 & x_{i}<-\eta \end{array}\right.$ |
|Quadratic |  $f(\mathbf{x})=\frac{1}{2} \mathbf{x}^{T} \mathbf{A} \mathbf{x}+\mathbf{b}^{T} \mathbf{x}+c$                    | $\frac{1}{2} \mathbf{u}^{T} \mathbf{A} \mathbf{u}+\mathbf{b}^{T} \mathbf{u}+c+\frac{1}{2}\|\mathbf{u}-\mathbf{x}\|^{2}$|$(\mathbf{A}+\mathbf{I})^{-1}(\mathbf{x}-\mathbf{b})$|

## Newton Method

### Application

For functions with easy-to-calculate Hessian $\nabla^{2} f(.)$.

### Algorithm

$$
\text{Use local quadratic approximation: }\\
g(x)=f\left(x_{0}\right)+\nabla f\left(x_{0}\right)^{T}\left(x-x_{0}\right)+\frac{1}{2}\left(x-x_{0}\right)^{T} \nabla^{2} f\left(x_{0}\right)\left(x-x_{0}\right)\\
\text{update rule:}\\
x_{+}=x_{0}-\left[\nabla^{2} f\left(x_{0}\right)\right]^{-1} \nabla f\left(x_{0}\right)\\
\text{Where} \left[\nabla^{2} f\left(x_{0}\right)\right]^{-1} \text{is the step size}
$$

### Properties

+ $+$ Affine invariance: Suppose $g(y)=f(A y),$ where $x=A y$ for invertible $A$
  $$
  \begin{aligned}
  \nabla g(y) &=A^{T} \nabla f(A y) \\
  \nabla^{2} g(y) &=A^{T} \nabla^{2} f(A y) A \\
  y_{+} &=y-\left[\nabla^{2} g(y)\right]^{-1} \nabla g(y) \\
  A y_{+} &=A y-A A^{-1}\left(\nabla^{2} f(A y)\right)^{-1} A^T \nabla g(y)\\
  A y_{+} &=A y-\left(\nabla^{2} f(A y)\right)^{-1} \nabla f(A y)
  \end{aligned}
  $$

+ $+$ can converge with fewer iterations
+ $-$ each iteration more expensive

