# Optimization Algorithms

## Subgradient Basics

$$
f(y)-f(x) \ge g^T(y-x) \quad \text{ [Subgradient] }\\
\partial f(x) = \text{set}(g) \quad \text{ [Subdifferential] }
$$

###  Properties

+ normally defined for convex function, definition works for non-convex function, but subgradients may not exist for non-convex functions
+ negative subgradient is not always a descent direction, i.e. $f(x-\eta g) \leq f(x)$ Not always true (always true if differentiable).
+ negative subgradient is ==descent direction for distance to optimal point==

- $\partial f(x)=\{\nabla f(x)\}$ if $f$ is differentiable at $x$
- Optimality conditions
  + Unconstrained convex $f(x)$: if $0 \in \partial f(\hat{x})$
  + Constrained convex, KKT stationarity: $0 \in \partial f(x)+\sum_{i=1}^{m} u_{i} \partial h_{i}(x)+\sum_{j=1}^{r} v_{j} \partial \ell_{j}(x)$ 
  + $x$ is optimal (minimizes $f$) iff. there is no descent direction for $f$ at $x$

### Basic rules

- scaling: $\partial(\alpha f)=\alpha \partial f($ for $\alpha>0)$
- summation + linearity (from set relationship): $\partial\left(a_{1} f_{1}+a_{2} f_{2}\right)=a_{1} \partial f_{1}+a_{2} \partial f_{2}$
- Affine composition: if $g(x)=f(A x+b)$, then $\partial g(x)=A^{T} \partial f(A x+b)$
- chain rule: 
  - $\partial(\phi \circ f)(\bar{x})=\{\alpha u||(\alpha, u) \in \partial \phi(f(\bar{x})) \times \partial f(\bar{x})\}$
  - suppose $f$ is convex, and $g$ is differentiable, nondecreasing, and convex. Let $h=g \circ f,$ then

$$
\partial h(\boldsymbol{x})=g^{\prime}(f(\boldsymbol{x})) \partial f(\boldsymbol{x})
$$

- composition: suppose $f(\boldsymbol{x})=h\left(f_{1}(\boldsymbol{x}), \cdots, f_{n}(\boldsymbol{x})\right),$ where $f_{i}$ 's
  are convex, and $h$ is differentiable, nondecreasing, and convex. Let $\boldsymbol{q}=\left.\nabla h(\boldsymbol{y})\right|_{\boldsymbol{y}=\left[f_{1}(\boldsymbol{x}), \cdots, f_{n}(\boldsymbol{x})\right]},$ and $\boldsymbol{g}_{i} \in \partial f_{i}(\boldsymbol{x}) .$ Then

$$
q_{1} \boldsymbol{g}_{1}+\cdots+q_{n} \boldsymbol{g}_{n} \in \partial f(\boldsymbol{x})
$$

- point-wise maximum: if $f(x)=\max _{1 \leq i \leq k} f_{i}(x),$ then ( convex hull of union of subdifferentials of 'active' functions at $x$)

$$
\partial f(\boldsymbol{x})=\underbrace{\operatorname{conv}\left\{\bigcup\left\{\partial f_{i}(\boldsymbol{x}) \mid f_{i}(\boldsymbol{x})=f(\boldsymbol{x})\right\}\right\}}_{\text {convex hull of subdifferentials of all active functions }}
$$

+ point-wise supremum: $\quad$ if $f(x)=\sup _{\alpha \in \mathcal{F}} f_{\alpha}(x),$ then

$$
\partial f(\boldsymbol{x})=\text { closure }\left(\operatorname{conv}\left\{\bigcup\left\{\partial f_{\alpha}(\boldsymbol{x}) \mid f_{\alpha}(\boldsymbol{x})=f(\boldsymbol{x})\right\}\right\}\right)
$$

### Examples

+ 1-Norm
  + $$\begin{aligned}
    &f(x)=\|x\|_{1}=\sum_{i=1}^{n}\left|x_{i}\right|\\
    &g \in \partial\|x\|_{1} \text { if either } g_{i}=\operatorname{sign}\left(x_{i}\right) \text { if } x_{i} \neq 0, \text { or } g_{i} \in[-1,1] \text { if } x_{i}=0
    \end{aligned}$$
+ $f(x)=\max \left\{f_{1}(x), f_{2}(x)\right\},$ where $f_{1}, f_{2}$ are differentiable.

$$
\begin{array}{l}
f_{1}(x)>f_{2}(x) \Rightarrow g=\nabla f_{1}(x) \\
f_{1}(x)<f_{2}(x) \Rightarrow g=\nabla f_{2}(x) \\
f_{1}(x)=f_{2}(x) \Rightarrow g \in \operatorname{conv}\left\{\nabla f_{1}(x), \nabla f_{2}(x)\right\}
\end{array}
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

|                            | Objective function                                           | argmin{}                                                     | Prox                                                         |
| -------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Projected Gradient Descent | $\min _{x \in C} g(x) \Longleftrightarrow \min _{x \in \mathbb{R}^{n}} g(x)+I_{C}(x)$ | $\frac{1}{2 t}\|x-z\|_{2}^{2}+I_{C}(z)= \|x-z\|_{2}^{2}$     | $P_c$                                                        |
| Lasso                      | $\min _{\beta}\|y-X \beta\|^{2}+\lambda\|\beta\|_{1}$        | $\left\{\frac{1}{2 \eta}\|\beta-z\|^{2}+\lambda\|z\|_{1}\right\}$ | $\left\{\begin{array}{ll}\beta_{i}-\eta \lambda & \text { if } \beta_{i}>\eta \lambda \\\beta_{i}+\eta \lambda & \text { if } \beta_{i}<-\eta \lambda \\0 & \text { else }\end{array}\right.$ |
| One-norm                   | $h(x)=\|x\|_{1}=\sum\left|x_{i}\right|$                      |                                                              | soft thresholding operator: $z_{i}=\left\{\begin{array}{ll}x_{i}-1 & x_{i}>\eta \\0 & -\eta \leqslant x_{i} \leqslant \eta \\x_{i}+1 & x_{i}<-\eta \end{array}\right.$ |
| Quadratic                  | $f(\mathbf{x})=\frac{1}{2} \mathbf{x}^{T} \mathbf{A} \mathbf{x}+\mathbf{b}^{T} \mathbf{x}+c$ | $\frac{1}{2} \mathbf{u}^{T} \mathbf{A} \mathbf{u}+\mathbf{b}^{T} \mathbf{u}+c+\frac{1}{2}\|\mathbf{u}-\mathbf{x}\|^{2}$ | $(\mathbf{A}+\mathbf{I})^{-1}(\mathbf{x}-\mathbf{b})$        |

## Newton Method

### Application

For functions with easy-to-calculate Hessian $\nabla^{2} f(.)$.

### Idea

$$
\text{Use local quadratic approximation: }\\
g(x)=f\left(x_{0}\right)+\nabla f\left(x_{0}\right)^{T}\left(x-x_{0}\right)+\frac{1}{2}\left(x-x_{0}\right)^{T} \nabla^{2} f\left(x_{0}\right)\left(x-x_{0}\right)\\
\text{update rule:}\\
x=x_{0}-\left[\nabla^{2} f\left(x_{0}\right)\right]^{-1} \nabla f\left(x_{0}\right)\\
\text{Where} \left[\nabla^{2} f\left(x_{0}\right)\right]^{-1} \text{is the step size}
$$

### Algorithm

given a starting point $x \in \operatorname{dom} f,$ tolerance $\epsilon>0$. repeat

1. Compute the Newton step and decrement.

$$
\Delta x_{\mathrm{nt}}:=-\nabla^{2} f(x)^{-1} \nabla f(x) ; \quad \lambda^{2}:=\nabla f(x)^{T} \nabla^{2} f(x)^{-1} \nabla f(x)
$$

2. Stopping criterion. quit if $\lambda^{2} / 2 \leq \epsilon$.
3. Line search. Choose step size $t$ by backtracking line search.
4. Update. $x:=x+t \Delta x_{\text {nt. }}$

+ $\lambda(x)$ Newton decrement: a measure of the proximity of $x$ to $x^{\star}$, is affine invariant

### Affine Invariance

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

+ Bounds of convergence are not affine invariant, depend on m, M, L..


### Convergence

#### General case: 

Fixed step size $\eta = 1$ Pure Newton Method, may not converge

#### Assume: m-Strongly convex + M-smooth + L-Lipschitz Hessian with Backtracking line search

+ Assumptions: $m I \leq \nabla^{2} f(x) \leq M I \quad \forall x$, $|\nabla^{2} f(x_1)-\nabla^{2} f(x_2)|\le L|x_1-x_2|$

+ Two stage convergence: $\alpha \in\left(0, m^{2} / L\right), \gamma>0$
  + damped Newton phase $\left(\|\nabla f(x)\|_{2} \geq \alpha\right)$ 
    + most iterations require backtracking steps 
    + function value decreases by at least $\gamma$,  $f\left(x_{+}\right)-f(x) \leq-\gamma$ or $f(x_k)-f^*\le f(x_0)-f^*-\gamma k$
    + Linear decrement/ log-sublinear convergence
    + if $p^{\star}>-\infty$, this phase ends after at most $\left(f\left(x^{(0)}\right)-p^{\star}\right) / \gamma$ iterations
  + (pure) quadratically convergent phase $\left(\|\nabla f(x)\|_{2}<\alpha\right)$
    + all iterations use step size $\eta=1$
    + gradient exponential decrement / $\|\nabla f(x)\|_{2}$ converges to zero quadratically: if $\left\|\nabla f\left(x^{(k)}\right)\right\|_{2}<\eta,$ then $\frac{L}{2 m^{2}}\left\|\nabla f\left(x_{+}\right)\right\| \leq\left(\frac{L}{2 m^{2}}\|\nabla f(x)\|\right)^{2} \le 1 \quad [\frac{L}{2 m^{2}}\text{ guarantees} \le 1]$
    + $f(x_k)-f^* \le \frac{2 m^{3}}{L^{2}}\left(\frac{1}{2}\right)^{k-k_{0}+1}$
  
+ Overall number of steps to reach $f(x)-f^{*} \leq \varepsilon$: 

  + $$
    \frac{f\left(x_{0}\right)-f^{*}}{\gamma}+\log \log \left(\frac{\varepsilon_{0}}{\varepsilon}\right)
    $$

  + **Quadratic convergence:** $\log \log \frac{\varepsilon_{0}}{\varepsilon} \approx$ constant, because it changes very slowly. Thus total number of steps ~ constant

  + Assume $\left|\epsilon_{0}\right|=\left|x^{*}-x_{0}\right|<1$

  + $\gamma$, $\epsilon_0$ depend on $m$, $L$. 

  + Compare to GD Linear convergence: $\log \left(\frac{1}{\varepsilon}\right)$ 

+ $+$ can converge with fewer iterations

+ $-$ each iteration more expensive

#### Assume: Self-Concordance

+ Definition:

  + In 1d $f: \mathbb{R} \rightarrow \mathbb{R}$: $\left|f^{\prime \prime \prime}(x)\right| \leq 2\left[f^{\prime \prime}(x)\right]^{\frac{3}{2}}$
  + In nd $f: \mathbb{R}^{n} \rightarrow \mathbb{R}$: if every 1d projection of the function is self-concordant

+ Examples:

  + $g(x)=-x^{p}$ for $0<p \leq 1$
    $g(x)=-\log x$
    $g(x)=x \log x$
    $g(x)=x^{p}$ for $-1 \leq p \leq 0$
    $g(x)=(a x+b)^{2} / x$
    Linear and quadratic functions

+ Properties:

  + Operations that preserve self-concordance: Linear combination, affine transformation (affine invariant)
  + If $f$ is self-concordant and the domain of $f$ contains no straight line (infinite in both directions), then $f^{\prime \prime}$ is non-singular (determinant vanishes).

+ Convergence for self-concordant $f$:

  + Newton with $B T L S(\alpha, \beta)$ reaches $\varepsilon$ -optimality in
    $$
    c(\alpha, \beta)\left(f\left(x_{0}\right)-f *\right)+\log \log \frac{1}{\varepsilon}
    $$

  + No $\epsilon_0$ here, thus not depending on the constants, convergence does not depend on unknown constants, gives affine invariant bound

## Quasi Newton Methods

### Idea

For case when finding and inverting Hessian is infeasible

+ Sequential approximation of inverse Hessian, using only gradient information, limited memory
+ Super-linear convergence when strong convexity holds plus some extra assumptions

### Requirements

1. $s_t = -B_t^{-1}\nabla f(x_t)$ [From Newton Update]
2. $s_t = B_{t+1}^{-1} y_t$ [Secant equation, from second-order Taylor expansion]

#### Update

+ From Newton to quasi-Newton: $x_{+} \leftarrow x-\left[\nabla^{2} f\left(x\right)\right]^{-1} \nabla f\left(x\right) \Rightarrow s_t = -B_t^{-1}\nabla f(x_t)$

#### Secant equation

##### Derivation

+ Second order Taylor Expansion$f_{t}(\boldsymbol{x}):=f\left(\boldsymbol{x}^{t+1}\right)+\left\langle\nabla f\left(\boldsymbol{x}^{t+1}\right), \boldsymbol{x}-\boldsymbol{x}^{t+1}\right\rangle+\frac{1}{2}\left(\boldsymbol{x}-\boldsymbol{x}^{t+1}\right)^{\top} \boldsymbol{H}_{t+1}\left(\boldsymbol{x}-\boldsymbol{x}^{t+1}\right)$

+ $\nabla f_{t}(\boldsymbol{x})=\nabla f\left(\boldsymbol{x}^{t+1}\right)+\boldsymbol{H}_{t+1}\left(\boldsymbol{x}^t-\boldsymbol{x}^{t+1}\right)$

Requirement: 

+ $\begin{aligned}
  \nabla f_{t}\left(\boldsymbol{x}^{t}\right) &=\nabla f\left(\boldsymbol{x}^{t}\right) \\
  \nabla f_{t}\left(\boldsymbol{x}^{t+1}\right) &=\nabla f\left(\boldsymbol{x}^{t+1}\right)
  \end{aligned}$

+ $$\begin{array}{r}
  \nabla f\left(\boldsymbol{x}^{t+1}\right)+\boldsymbol{H}_{t+1}\left(\boldsymbol{x}^{t}-\boldsymbol{x}^{t+1}\right)=\nabla f\left(\boldsymbol{x}^{t}\right) \\
  \Longleftrightarrow \underbrace{\boldsymbol{H}_{t+1}\left(\boldsymbol{x}^{t+1}-\boldsymbol{x}^{t}\right)=\nabla f\left(\boldsymbol{x}^{t+1}\right)-\nabla f\left(\boldsymbol{x}^{t}\right)}_{\text {secant equation }}
  \end{array}$$

+ $$\boldsymbol{H}_{t+1}^{-1} \underbrace{\left(\nabla f\left(\boldsymbol{x}^{t+1}\right)-\nabla f\left(\boldsymbol{x}^{t}\right)\right)}_{=\boldsymbol{y}_{t}}=\underbrace{\boldsymbol{x}^{t+1}-\boldsymbol{x}^{t}}_{=: \boldsymbol{s}_{t}}$$

+ Convex function has positive semidefinite Hessian: $\boldsymbol{s}_{t}^{\top} \boldsymbol{y}_{t}=\boldsymbol{y}_{t}^{\top} \boldsymbol{H}_{t+1}^{-1} \boldsymbol{y}_{t}>0$
+ Has infinite number of Hessians as solution: $n$ equality constraints with $O(n^2)$ degree of freedom in Hessian
+ Different methods offer different choice of solution
+ Approximate $B\approx \nabla^{2} f(x),$

### Rank-1 Update

$$
\begin{array}{c}B_{+}=B+a u u^{T} &[a u u^{T}\text{ is symmetric, rank 1 matrix}, a \text{ is scalar}, u \text{ vector}] \\B_{+} s=y \Rightarrow\left(a u^{T} s\right) u=y-B s &[a, u^{T} s\text{ are scalar}] \end{array}
$$

$$
B_{+} \leftarrow B+\frac{(y-B s)(y-B s)^{T}}{(y-B s)^{T} s}\\

s_+=-B_+^{-1}\nabla f(x)\\
x_{k+2}=x_{k+1}+s_{k+1} \eta_{k+1}
$$

+ by Sherman, Morrison, Woodbury (SMW) theorem: $\left(A+u v^{T}\right)^{-1}=A^{-1}-\frac{A^{-1} u v^{T} A^{-1}}{1+v^{T} A^{-1} u}$, where $\frac{A^{-1} u v^{T} A^{-1}}{1+v^{T} A^{-1} u}$ is rank=1, we can update $B_+^{-1}$ from $B^{-1}$ directly with low cost
+ **Problem**: if $(y-B s)^{T} s<0$, $B_+$ may not be positive semidefinite, $B_+$ needs to be symmetric and positive semidefinite for convex function

### Rank-2 Update

$$
\begin{array}{c}B_{+}=B+a u u^{T}+b v v^{T} &[a u u^{T}+bu u^{T}\text{ is  rank 2} ] \\B_{+} s=y \Rightarrow\left(a u^{T} s\right) u=y-B s &[a, u^{T} s\text{ are scalar}] \end{array}
$$


​				

#### Broyden-Fletcher-Goldfarb-Shanno (BFGS) method

