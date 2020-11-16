# Optimization Exam 2
## Basic notes
1. $Q^{-1}$ is symmetric if $Q$ is symmetric

### Convex conditions
+ 0-st order condition: $f(\theta x+(1-\theta) y) \leq \theta f(x)+(1-\theta) f(y)$, extension $f(\mathbf{E} z) \leq \mathbf{E} f(z)$ for any random variable $z$
+ 1st-order condition: differentiable $f$, $f(y) -f(x)\geq \nabla f(x)^{T}(y-x) \quad \text { for all } x, y \in \operatorname{dom} f$
+ 2-nd order condition: $\nabla^{2} f(x) \succeq 0 \quad \text { for all } x \in \operatorname{dom} f$
+ $\forall x, y(\nabla f(x)-\nabla f(y))^{T}(x-y) \geq 0$

### Convex program definition
standard form convex optimization problem

$$
\begin{array}{ll}
\operatorname{minimize} & f_{0}(x) \\
\text { subject to } & f_{i}(x) \leq 0, \quad i=1, \ldots, m \\
& a_{i}^{T} x=b_{i}, \quad i=1, \ldots, p
\end{array}
$$

$\bullet f_{0}, f_{1}, \ldots, f_{m}$ are convex; equality constraints are affine
- problem is quasiconvex if $f_{0}$ is quasiconvex (and $f_{1}, \ldots, f_{m}$ convex)


### Operations that preserve convexity
+ $\alpha f$- nonnegative multiple: $\alpha f$ is convex if $f$ is convex, $\alpha \geq 0$
+  $f_{1}+f_{2}$ - sum: $f_{1}+f_{2}$ convex if $f_{1}, f_{2}$ convex (extends to infinite sums, integrals)
+ $f(A x+b)$- composition with affine function: $f(A x+b)$ is convex if $f$ is convex
+ Convex proven from operations that preserves convexity
  - log barrier for linear inequalities : $f(x)=-\sum_{i=1}^{m} \log \left(b_{i}-a_{i}^{T} x\right)$
  - norm of affine function: $f(x)=\|A x+b\|$
  - point-wise maximum/ supremum: $f(x)=\max \left\{f_{1}(x), \ldots, f_{m}(x)\right\}$, or $f(x):=\sup _{i} f_{i}(x)$
    - piecewise-linear function: $f(x)=\max _{i=1, \ldots, m}\left(a_{i}^{T} x+b_{i}\right)$ is convex
    - sum of $r$ largest components of $x \in \mathbf{R}^{n}$ :$f(x)=x_{[1]}+x_{[2]}+\cdots+x_{[r]}$
    - support function of a set $C: S_{C}(x)=\sup _{y \in C} y^{T} x$ is convex
    - distance to farthest point in a set $C$ :$f(x)=\sup _{y \in C}\|x-y\|$
    - maximum eigenvalue of symmetric matrix: for $X \in \mathbf{S}^{n}$,$\lambda_{\max }(X)=\sup _{\|y\|_{2}=1} y^{T} X y$
+ Composition function, look at 2-nd derivative
  - $f(x)=h(g(x))$: $f^{\prime \prime}(x)=h^{\prime \prime}(g(x)) g^{\prime}(x)^{2}+h^{\prime}(g(x)) g^{\prime \prime}(x)$, or $f^{\prime \prime}(x)=g^{\prime}(x)^{T} \nabla^{2} h(g(x)) g^{\prime}(x)+\nabla h(g(x))^{T} g^{\prime \prime}(x)$
  - $\exp g(x)$ is convex if $g$ is convex
  - $1 / g(x)$ is convex if $g$ is concave and positive
  - $\sum_{i=1}^{m} \log g_{i}(x)$ is concave if $g_{i}$ are concave and positive
  - $\log \sum_{i=1}^{m} \exp g_{i}(x)$ is convex if $g_{i}$ are convex
- minimize convex $f$ over certain variables in a convex set $C$: $g(x)=\inf _{y \in C} f(x, y)$ is convex
- Perspective of convex $f$ is convex: $g(x, t)=t f(x / t)$
- ==Conjugate is always convex== no matter $f$: $f^{*}(y)=\sup _{x \in \operatorname{dom} f}\left(y^{T} x-f(x)\right)$
- ==Lagrange dual function is always concave==: $g(\boldsymbol{\lambda}, \boldsymbol{\nu})=\inf _{\boldsymbol{x} \in \mathbb{R}^{N}}\left(f_{0}(\boldsymbol{x})+\sum_{m=1}^{M} \lambda_{m} f_{m}(\boldsymbol{x})+\sum_{p=1}^{P} \nu_{p} h_{p}(\boldsymbol{x})\right)$

### Example of Convex functions
+ Total weight of weighted shortest path $f(w)$ is concave in weight $w$, $f(w)=\min _{p}\left(\sum_{e \in p} w_{e}\right)$ (minimization of piecewise linear function in weight - concave)
+ convex in $\mathbb{R}$
  - exponential: $e^{a x},$ for any $a \in \mathbf{R}$
  - powers: $x^{\alpha}$ on $\mathbf{R}_{++},$ for $\alpha \geq 1$ or $\alpha \leq 0$
  - powers of absolute value: $|x|^{p}$ on $\mathbf{R},$ for $p \geq 1$
  - negative entropy: $x \log x$ on $\mathbf{R}_{++}$
+ concave in $\mathbb{R}$
  - powers: $x^{\alpha}$ on $\mathbf{R}_{++},$ for $0 \leq \alpha \leq 1$
  - logarithm: $\log x$ on $\mathbf{R}_{++}$
+ convex in $\mathbb{R}^n$ and $\mathbb{R}^{m\times n}$
  - Norms: $\|x\|_{p}=\left(\sum_{i=1}^{n}\left|x_{i}\right|^{p}\right)^{1 / p} \text { for } p \geq 1 ;\|x\|_{\infty}=\max _{k}\left|x_{k}\right|$
  - Spectral (maximum singular value) norm: $f(X)=\|X\|_{2}=\sigma_{\max }(X)=\left(\lambda_{\max }\left(X^{T} X\right)\right)^{1 / 2}$
  - Any line along a convex function: 
  $f: \mathbf{R}^{n} \rightarrow \mathbf{R}$ is convex if and only if the function $g: \mathbf{R} \rightarrow \mathbf{R}$
$$
g(t)=f(x+t v), \quad \operatorname{dom} g=\{t \mid x+t v \in \operatorname{dom} f\}
$$
is convex $($ in $t)$ for any $x \in \operatorname{dom} f, v \in \mathbf{R}^{n}$
+ Affine function is both convex and concave:  $f(x)=a^{T} x+b$, $f(X)=\operatorname{tr}\left(A^{T} X\right)+b=\sum_{i=1}^{m} \sum_{j=1}^{n} A_{i j} X_{i j}+b$
+ Non-linear (proven from 2-nd order condition)
  - Quadratic: $f(x)=(1 / 2) x^{T} P x+q^{T} x+r$ convex if $P \succeq 0$
  - Least squares: $f(x)=\|A x-b\|_{2}^{2}$ convex $\forall A$
  - quadratic-over-linear: $f(x, y)=x^{2} / y$ convex for $y>0$
  - log-sum-exp is convex: $f(x)=\log \sum_{k=1}^{n} \exp x_{k}$
  - geometric mean: $f(x)=\left(\prod_{k=1}^{n} x_{k}\right)^{1 / n} \text { on } \mathbf{R}_{++}^{n}+$
  - sub-level set of convex functions are convex (not converse): $C_{\alpha}=\{x \in \operatorname{dom} f \mid f(x) \leq \alpha\}$
  - ==$f$ is convex if and only if $\text{epi} f$ is a convex set==: $\text { epi } f=\left\{(x, t) \in \mathbf{R}^{n+1} \mid x \in \operatorname{dom} f, f(x) \leq t\right\}$

### Theorems
+ Separating hyperplane theorem: if $C$ and $D$ are nonempty disjoint convex sets, there exist $a \neq 0, b$ s.t.
$$a^{T} x \leq b \text { for } x \in C, \quad a^{T} x \geq b \text { for } x \in D$$
+ Supporting hyperplane theorem: if C is convex, then there exists a
supporting hyperplane at every boundary point of C:
$$
\left\{x \mid a^{T} x=a^{T} x_{0}\right\}
$$
where $a \neq 0$ and $a^{T} x \leq a^{T} x_{0}$ for all $x \in C$

## Week 6,7 - SDP
### Symmetric matrices
  + Symmetric matrices always have ==eigenvectors that are orthogonal to each other, but may have negative eigenvalues==
  + If $A^{-1}$ exists, it is symmetric if and only if $A$ is symmetric.
  + The sum and difference of two symmetric matrices is again symmetric
  + This is not always true for the product: given symmetric matrices A and V, then ==AB is symmetric if and only if AB=BA==
  + A symmetric real matrix $A$ is diagonalizable and can be written as $U \Lambda U^{T},$ where $\Lambda=\operatorname{diag}\left(\lambda_{1}, \ldots, \lambda_{n}\right)$ is
a matrix with $A$ 's eigenvalues along the main diagonal and zeros elsewhere, and $U$ is an orthogonal matrix
  + Every quadratic form $q$ on $\mathbb{R}^{n}$ can be uniquely written in the form $q(\mathbf{x})=\mathbf{x}^{\top} A \mathbf{x}$ with a symmetric $n \times n$ matrix $A$. $q\left(x_{1}, \ldots, x_{n}\right)=\sum_{i=1}^{n} \lambda_{i} x_{i}^{2}$
### Positive Semidefinite Matrices (PSD)
#### Definitions
  + Always square and symmetric $M=M^T$
  + $x^{\mathrm{T}} M x \geq 0$ for all $x \in \mathbb{R}^{n}$
  + positive definite: $x^{\top} M x>0$ for all $x \in \mathbb{R}^{n} \backslash \mathbf{0}$
  + $A$ Hermitian (equal to its own conjugate transpose, i.e. real) $\Longleftrightarrow a_{i j}=\bar{a}_{j i}$ . Hermitian matrix M is:
    + positive (semi) definite if and only if all of its eigenvalues are (non-negative) positive.
    + indefinite if and only if it has both positive and negative eigenvalues.
    +  positive semidefinite if and only if it can be decomposed as a product $M=B^{*} B$
  + <mark> Real $B$ </mark>,  $M=B^{\top} B$ is always positive semidefinite, not if $B$ is complex.

#### Properties
+ $M \geq N \text { if } M-N \geq 0$
+ Every positive definite matrix is invertible and its inverse is also positive definite. If $M \geq N>0$ then $N^{-1} \geq M^{-1}>0$ Moreover, by the min-max theorem, the $k$ th largest eigenvalue of $M$ is greater than the $k$ th largest eigenvalue of $N$.
+ Operation that preserves PSD: 
  + addition, some multiplication (MNM, NMN, or MN=NM)
  + Hadamard product $M \circ N = \sum M_{ij}N_{ij}\geq 0$
+ $tr(M)\ge 0$
+ Convexity: if $M$ and $N$ are positive semidefinite, then for any $\alpha$ between 0 and 1, ==$\alpha M+(1-\alpha) N$ is also positive semidefinite==
+ If $M>0$ is real, then there is a $\delta>0$ such that $M>\delta I,$ where $I$ is the identity matrix.
+ $A \le 0$ then $-A\ge 0$

### Semidefinite program
#### Definition
+ Linear objective function
+ Positive semidefinite constraints
+ Convex
#### Original Formulation
$$
\begin{array}{ll}
\underset{x^{1}, \ldots, x^{n} \in \mathbb{R}^{n}}{\min } & \sum_{i, j \in[n]} c_{i, j}\left(x^{i} \cdot x^{j}\right) \\
\text { subject to } & \sum_{i, j \in[n]} a_{i, j, k}\left(x^{i} \cdot x^{j}\right) \leq b_{k} \text { for all } k
\end{array}
$$
where the $c_{i, j}, a_{i, j, k},$ and the $b_{k}$ are real numbers and $x^{i} \cdot x^{j}$ is the dot product of $x^{i}$ and $x^{j}$
+  $n \times n$ matrix $M \succeq 0$ if it is the Gramian matrix of some vectors (i.e. if there exist vectors $x^{1}, \ldots, x^{n}$ such that $m_{i, j}=x^{i} \cdot x^{j}$ for all $\left.i, j\right)$. 

#### Matrix Formulation
$$
\begin{aligned}
\min _{X \in \mathbb{S}^{n}} &\langle C, X\rangle_{\mathrm{S}^{n}} \\
\text { subject to } &\left\langle A_{k}, X\right\rangle_{\mathbb{S}^{n}} \leq b_{k}, \quad k=1, \ldots, m \\
& X \succeq 0
\end{aligned}
$$
+ $C_{i,j}=\frac{c_{i, j}+c_{j, i}}{2}$ symmetric $n\times n$
+ $A_{k,i,j}=\frac{a_{i, j, k}+a_{j, i, k}}{2}$  symmetric $n\times n$

##### Dual
$$
\begin{aligned}
\max _{y \in \mathbb{R}^{m}} &\langle b, y\rangle_{\mathbb{R}^{m}} \\
\text { subject to } & \sum_{i=1}^{m} y_{i} A_{i} = C
\\
& y_i \preceq 0
\end{aligned}
$$


#### Equality Formulation
If add slack variables properly:
$$
\begin{aligned}
\min _{X \in \mathbb{S}^{n}} &\langle C, X\rangle_{\mathbb{S}^{n}} \\
\text { subject to } &\left\langle A_{k}, X\right\rangle_{\mathrm{S}^{n}}=b_{k}, \quad k=1, \ldots, m \\
& X \succeq 0
\end{aligned}
$$
##### Dual
$$
\begin{aligned}
\max _{y \in \mathbb{R}^{m}} &\langle b, y\rangle_{\mathbb{R}^{m}} \\
\text { subject to } & \sum_{i=1}^{m} y_{i} A_{i} \preceq C
\end{aligned}
$$

#### Schur Complement
$$
X=\left[\begin{array}{ll}
A & B \\
B^{\top} & C
\end{array}\right]
$$

+ If $X$ symmetric and $C$ invertible:
+ $X \succ 0 \Leftrightarrow A \succ 0, X / A=C-B^{\top} A^{-1} B \succ 0$
+ $X \succ 0 \Leftrightarrow C \succ 0, X / C=A-B C^{-1} B^{\top} \succ 0$
+ If $A \succ 0,$ then $X \succeq 0 \Leftrightarrow X / A=C-B^{\top} A^{-1} B \succeq 0$
+ If $C \succ 0,$ then $X \succeq 0 \Leftrightarrow X / C=A-B C^{-1} B^{\top} \succeq 0$
Generalized Schur Complement
+ $X \succeq 0 \Leftrightarrow A \succeq 0, C-B^{\top} A^{g} B \succeq 0,\left(I-A A^{g}\right) B=0$
+ $X \succeq 0 \Leftrightarrow C \succeq 0, A-B C^{g} B^{\top} \succeq 0,\left(I-C C^{g}\right) B^{\top}=0$
+ where $A^{g}$ denotes the generalized inverse of $A$.

### Examples of SDP

#### 1. Linear-fractional program
+ Is a <mark>quasiconvex problem</mark>
+ Equivalent to SDP
$$
\begin{array}{l}
\text { minimize } \frac{\left(c^{T} x\right)^{2}}{d^{T} x} \\
\text { subject to } A x+b \geq 0
\end{array}
$$
where $d^{T} x>0$ whenever $A x+b \geq 0$
$$
\begin{aligned}
&\text { minimize } t\\
&\text { subject to }\left[\begin{array}{ccc}
\operatorname{diag}(A x+b) & 0 & 0 \\
0 & t & c^{T} x \\
0 & c^{T} x & d^{T} x
\end{array}\right] \succeq 0
\end{aligned}
$$

#### 2. QCQP
$$\begin{array}{ll}\operatorname{minimize} & \frac{1}{2} x^{\mathrm{T}} P_{0} x+q_{0}^{\mathrm{T}} x + c_0\\ \text { subject to } & \frac{1}{2} x^{\mathrm{T}} P_{i} x+q_{i}^{\mathrm{T}} x+r_{i} \leq 0 \quad \text { for } i=1, \ldots, m \\ & A x=b\end{array}$$
where $P_{0}, \ldots, P_{m}$ are $n \times n$ and $x \in \mathbf{R}^{n}$ 
+ If these matrices are neither positive nor negative semidefinite, the problem is non-convex. 
+ If $P_{1}, \ldots, P_{m}$ are all zero, then constraints are linear and the problem is a quadratic program.
+ ==If $P_{0}, \ldots, P_{m} = M_i^TM_i$ are all positive semidefinite, then the problem is convex. (SDP)==
$$
\begin{aligned}
&\text { minimize } t\\
&\text { subject to }\left[\begin{array}{cc}
I & M_0X \\
x^TM_0^T & -c_0-q_0^Tx +t \end{array}\right] \succeq 0\\
& \left[\begin{array}{cc}
I & M_iX \\
x^TM_i^T & -c_i-q_i^Tx \end{array}\right] \succeq 0
\end{aligned}
$$

#### 3. Sum of squares (SoS)
+ f is PSD: $f(x) \geq 0 \quad \forall x \in \mathbb{R}^{n}$
+ SoS decomposition: $f$ is PSD if $f=\sum_{i=1}^{s} g_{i}^{2}$, polynomials $g_{1}, \ldots, g_{s}$ (i.e. polynomial $g_i(x)$) A certificate for [$f$ is PSD].
+ Find semidefinite formulation:
 $g_i(x) = <g_i, x>$ where $g_i^j$ is the j'th coefficient of the polynomial. 
 $\sum g_i(x)^2=\sum x^Tg_i^Tg_ix = x^T[\sum g_i^Tg_i]x=x^TQx$, $Q \succeq 0$
+ $f$ is $\mathrm{SOS}$ if and only if there exists $Q$ such that
$$
\begin{array}{l}
Q \succeq 0 \\
f=z^{T} Q z
\end{array}
$$
+ Correspondence to coefficients of f ($q_i$):
  $q_0 = Q_{00}$
  $q_1 = Q_{10}+Q_{01}$
  $q_2 = Q_{20}+Q_{11}+Q_{02}$
+ Example optimization problem
$$
\begin{array}{l}
\min p(x), \quad s.t. \quad x \in \mathbb{R}\\
\max r, \quad s.t. \quad p(x)-r \ge 0, \forall x\in \mathbb{R}\\
\max r, \quad s.t. \quad p(x)-r=\sum (h_j(x))^2\\
\max r, \quad s.t. \quad p(x)-r= x^TQx, Q \succeq 0\\
\max r, \quad s.t. \quad p_0 = Q_{00}-r, p_1 = Q_{10}+Q_{01}....,Q \succeq 0\\
\end{array}
$$
+ Convexity
  + ==the sets of PSD and SOS polynomials are a convex cones;== i.e.,$f, g \text{ PSD } \quad \Longrightarrow \quad \lambda f+\mu g$ is $\mathrm{PSD}$ for all $\lambda, \mu \geq 0$
+ ==SOSs (easily verifiable) $\subset$ PSDs (NP hard)==

## Week 7 - Convex programming duality
### Lagrange duality
https://en.wikipedia.org/wiki/Duality_(optimization)

nonlinear programming problem in standard form
$$
\begin{aligned}
\operatorname{minimize} f_{0}(x)\\
\text { subject to } f_{i}(x) & \leq 0, i \in\{1, \ldots, m\} \\
h_{i}(x) &=0, i \in\{1, \ldots, p\}
\end{aligned}
$$
+ domain $\mathcal{D} \subset \mathbb{R}^{n}$ have non-empty interior
+ Lagrangian function $L: \mathbb{R}^{n} \times \mathbb{R}^{m} \times \mathbb{R}^{p} \rightarrow \mathbb{R}$ is defined as
$$
L(x, \lambda, \nu)=f_{0}(x)+\sum_{i=1}^{m} \lambda_{i} f_{i}(x)+\sum_{i=1}^{p} \nu_{i} h_{i}(x)
$$
+ $\lambda$ and $\nu$: the dual variables or Lagrange multiplier vectors
+ Lagrange dual function $g: \mathbb{R}^{m} \times \mathbb{R}^{p} \rightarrow \mathbb{R}$ is defined as
$$
g(\lambda, \nu)=\inf _{x \in \mathcal{D}} L(x, \lambda, \nu)
$$
+ $g$ is __concave__, even when the initial problem is not convex, because it is a point-wise infimum (min) of affine functions. 
+ ==If Slater's condition holds and the original problem is convex (also works for non-convex problem), then we have strong duality,== i.e. $d^{*}=\max _{\lambda \geq 0, \nu} g(\lambda, \nu)=\inf f_{0}=p^{*}$

### Weak duality
+ Weak duality: the value of the dual SDP $\le$ value of the primal: $g(\lambda, \nu) \leq p^{*}$

### Strong duality
+ sufficient condition for strong duality to hold for convex problem
+ Slater's condition (equivalent description):  
  + feasible region have an interior point 
  + strictly feasible: all constraints are satisfied and the nonlinear constraints are satisfied with strict inequalities
  + $f_i(x)<0, \quad \forall i$
  + Note: need strict inequality only over $h_i$ that are
not affine.
+ Every LP has strong duality, Strong duality breaks in LP only when both primal and dual are infeasible.
+ not every SDP satisfies strong duality
+ SDP has strong duality if: either primal or dual is strictly feasible (has interior points) (there exists $X_{0} \in \mathbb{S}^{n}, X_{0} \succ 0$ such that $\left.\left\langle A_{i}, X_{0}\right\rangle_{\mathrm{S}^{n}}=b_{i}, i=1, \ldots, m\right)$ or ( $\sum_{i=1}^{m}\left(y_{0}\right)_{i} A_{i} \prec C$ for some $\left.y_{0} \in \mathbb{R}^{m}\right)$.


### Examples
$$
\begin{aligned}
\operatorname{minimize} x^T\omega x\\
\text { s.t. } x_i^2=1 \forall i \\
L(x,\nu) = x^T\omega x+\sum\nu_i(x_i^2-1)\\
=x^T(\omega+diag(\nu))x -\mathbb{1}^T\nu\\
g(\nu)=\min_x L(x,\nu)\\
= -1^T\nu \text{ if }\omega+diag(\nu)\ge 0, -\infty \text{ else }\\
\text {choose minimum value that } \omega+diag(\nu)\ge 0 \text { holds: }\\
 \nu = -\lambda_{min}(\omega)1^T \\
 p^* \ge 1^T\lambda_{min}(\omega)1^T=n\lambda_{min}(\omega)
\end{aligned}
$$

### Duality and sensitivity
Perturbe with $u_i, v_i$:
$$
\begin{array}{ll}
\operatorname{minimize} & f_{0}(x) \\
\text { subject to } & f_{i}(x) \leq u_{i}, \quad i=1, \ldots, m \\
& h_{i}(x)=v_{i}, \quad i=1, \ldots, p
\end{array}
$$
$p^{\star}(u, v)$ as the optimal value of the perturbed problem. Assume strong duality: 
$$
\begin{aligned}
p^{\star}(0,0)=g\left(\lambda^{\star}, \nu^{\star}\right) & \leq f_{0}(x)+\sum_{i=1}^{m} \lambda_{i}^{\star} f_{i}(x)+\sum_{i=1}^{p} \nu_{i}^{\star} h_{i}(x) \\
& \leq f_{0}(x)+\lambda^{\star T} u+\nu^{\star T} v
\end{aligned}
$$
$$
p^{\star}(u, v) \geq p^{\star}(0,0)-\lambda^{\star T} u-\nu^{\star T} v
$$
#### Local sensitivity around (0,0):
$$
\lambda_{i}^{\star}=-\frac{\partial p^{\star}(0,0)}{\partial u_{i}}, \quad \nu_{i}^{\star}=-\frac{\partial p^{\star}(0,0)}{\partial v_{i}}
$$
__Proof__:
$$
\lim _{t \rightarrow 0} \frac{p^{\star}\left(t e_{i}, 0\right)-p^{\star}}{t}=\frac{\partial p^{\star}(0,0)}{\partial u_{i}}
$$
for $t>0$
$$
\frac{p^{\star}\left(t e_{i}, 0\right)-p^{\star}}{t} \geq-\lambda_{i}^{\star}
$$
while for $t<0$ we have the opposite inequality. Taking the limit $t \rightarrow 0,$ with $t>0$ yields
$$
\frac{\partial p^{\star}(0,0)}{\partial u_{i}} \geq-\lambda_{i}^{\star}
$$
while taking the limit with $t<0$ yields the opposite inequality, so we conclude that
$$
\frac{\partial p^{\star}(0,0)}{\partial u_{i}}=-\lambda_{i}^{\star}
$$

## Week 8 - KKT Condition
_Nonlinear_ problem:
Optimize $f(\mathbf{x})$
subject to
$$
\begin{array}{l}
f_{i}(\mathbf{x}) \leq 0 \\
h_{j}(\mathbf{x})=0
\end{array}
$$
### 1. Theorem
If 
1. _strong duality holds_
2. $f_i$ and $h_j$ are continuously differentiable

then [$x$ is primal opt, $\lambda, \nu$ are dual opt] iff. [$x, \lambda, \nu$ satisfy KKT]

### 2. KKT
1. Primal feasibility $f_{i}(x) \leq 0, \quad h_{i}(x)=0$
2. Dual feasibility $\lambda_i \ge 0$
3. Complementary slackness: $\lambda_i f_i(x) =0$
4. Stationarity: $\nabla L = \nabla f_{0}\left(x^{\star}\right)+\sum_{i=1}^{m} \nabla f_{i}\left(x^{\star}\right)^{T} \lambda_{i}^{\star}+\sum_{i=1}^{p} \nu_{i}^{\star} \nabla h_{i}\left(x^{\star}\right)=0$
Stationarity in practice: for tight primal:
$$-\nabla f_0 = \sum \lambda_i \nabla f_i$$
$\nabla f_i$ are n-dimensional.

+ two linear programming problems are dual to each other if and only if they share KKT conditions with the primal and dual feasibility conditions swapped.

### Approach to prove optimality
1. Check feasibility
2. With complementary slackness, find which constraint is loose:
  + For loose constraint: dual variable $\lambda_i = 0$, ignore for next steps
  + For tight constraint: use dual variable $\lambda_i \ge 0$ in next steps
3. Calculate stationarity $\frac{dy_0}{dx} + \sum \lambda_i \frac{dy_i}{dx} = 0 \in \mathbb{R}^n$

## Week 9 SVM
### Max Margin Classification
+ Margin (of a linear classifier/ hyperplane): min distance between any data point and hyperplane
+ Max margin classifier: the one with largest margin
+ Find Max margin classifier:
  + Classifier: $y^{(i)}\left(w^{\top} x^{(i)}-b\right)>0$
  + Rescale to make $y^{(i)}\left(w^{\top} x^{(i)}-b\right) \geq 1$
  + Calculate margin between planes $w^{\top} x-b=\pm 1$ is $\frac{2}{\|w\|}$
  + Task becomes:
  $$\begin{aligned}
&\min _{w, b} \quad\|w\|^{2}\\
&\text { s.t. } \quad y^{(i)}\left(w^{\top} x^{(i)}-b\right) \geq 1 \text { for all samples } i
\end{aligned}$$
  + __Support Vectors__: the best $(\widehat{w}, \hat{b})$ found
### Soft Margin
+ For data that is not separable, add a penalty for being wrong (but still allowed)
$$
\begin{aligned}
&\min _{w, b, \xi} \frac{1}{n^{n}} \sum_{i} \xi^{(i)}+\lambda\|w\|^{2}\\
&\text { s.t. } \quad y^{(i)}\left(w^{\top} x^{(i)}-b\right) \geq 1-\xi^{(i)} \text { for all samples } i\\
&\xi^{(i)} \geq 0 \text { for all samples } i
\end{aligned}
$$

### Dual of Soft Margin
$$
\begin{aligned}
&\begin{array}{ll}
\max _{c} & \sum_{i} c^{(i)}-\frac{1}{2} \sum_{i} \sum_{j} y^{(i)} c^{(i)}\left\langle x^{(i)}, x^{(j)}\right\rangle y^{(j)} c^{(j)} \\
\text { s.t. } & \sum_{i} c^{(i)} y^{(i)}=0
\end{array}\\
& 0 \leq c^{(i)} \leq \frac{1}{2 n \lambda} \quad \text { for all samples }
\end{aligned}
$$
+ Obtain primal optima (support vectors) from dual optima: $\widehat{w}=\sum \widehat{c}^{(i)} y^{(i)} x^{(i)}$, and  $\widehat{b}=w^{\top} x^{(i)}-y^{(i)}$ for any supporting $i$
+ $\widehat{c}^{(i)}\ne 0$ only if $i$ is a support vector

### Kernel Trick
+ Kernel: as a similarity measure
+ $\text { Replace all }\left\langle x^{(i)}, x^{(j)}\right\rangle \text { by } k\left(x^{(i)}, x^{(j)}\right), \text { where } k(\cdot, \cdot) \text { is the kernel function }$
+ $\max _{c} \sum_{i} c^{(i)}-\frac{1}{2} \sum_{i} \sum_{j} y^{(i)} c^{(i)} k\left(x^{(i)}, x^{(j)}\right) y^{(j)} c^{(j)}$
+ $k(\hat{w}, x)=\sum_{i} \widehat{c}^{(i)} y^{(i)} k\left(x^{(i)}, x\right)$
+ $\widehat{c}^{(i)}\ne 0$ only if $i$ is a support vector

### Kernel SVM

Non-linear classifiers
+ Quadratic Kernel: $k(a, b)=\left(a^{\top} b\right)^{2}$, equivalent to linear classifier over a larger set of features
  + $K(x, y)=\left(\sum_{i=1}^{n} x_{i} y_{i}+c\right)^{2}=\sum_{i=1}^{n}\left(x_{i}^{2}\right)\left(y_{i}^{2}\right)+\sum_{i=2}^{n} \sum_{j=1}^{i-1}\left(\sqrt{2} x_{i} x_{j}\right)\left(\sqrt{2} y_{i} y_{j}\right)+\sum_{i=1}^{n}\left(\sqrt{2 c} x_{i}\right)\left(\sqrt{2 c} y_{i}\right)+c^{2}$
  + Feature map is given by: $\varphi(x)=\left\langle x_{n}^{2}, \ldots, x_{1}^{2}, \sqrt{2} x_{n} x_{n-1}, \ldots, \sqrt{2} x_{n} x_{1}, \sqrt{2} x_{n-1} x_{n-2}, \ldots, \sqrt{2} x_{n-1} x_{1}, \ldots, \sqrt{2} x_{2} x_{1}, \sqrt{2 c} x_{n}, \ldots, \sqrt{2 c} x_{1}, c\right\rangle$

+ RBF Kernel: $K\left(\mathbf{x}, \mathbf{x}^{\prime}\right)=\exp \left(-\frac{\left\|\mathbf{x}-\mathbf{x}^{\prime}\right\|^{2}}{2 \sigma^{2}}\right)$,  feature space of the kernel has an infinite number of dimensions

## Week 9 Conjugate Function
+ Given function $f: \mathbb{R}^{n} \rightarrow \mathbb{R},$ its conjugate $f^{*}: \mathbb{R}^{n} \rightarrow \mathbb{R}$ is
$$
f^{*}(y)=\max _{x} y^{T} x-f(x)
$$

### Properties
+ Every function as a conjugate
+ $f^{*}$ always convex function because it is max of affine functions
+ Fenchel’s inequality: $f(x)+f^{*}(y) \geq y^{T} x$
+ If can be decoupled: $\text { If } f(u, v)=f_{1}(u)+f_{2}(v) \text { then } f^{*}(w, z)=f_{1}^{*}(w)+f_{2}^{*}(z)$
+ Double conjugate: 
  + $f^{* *}(z) \leq f(z)$, $f^{* *}(z)$ is always convex
  + if $f$ is __closed__ and convex:
    + $f^{* *}(z)=f(z)$ and $f(x)+f^{*}(y)=x^{T} y$ 
    + $f$ and $f^{*}$ are inverse of each other, i.e. $y=\nabla f(x)$<=> $x=\nabla f^*(y)$
  + If f is not convex, 
    + $f^{**}$ is the convex envelope of $f$, or the largest convex function smaller than $f$, or $g(x) \leq f(x) \forall x$, then $g(x) \leq f^{* *}(x)$
    + $y \in \partial f(x)$<=> $x \in \partial f^*(y)$
+ If $f$ is convex and $\bar{y} \in \partial f(\bar{x})$, then $f^*(\bar{y})=\bar{y}^{T} \bar{x}-f(\bar{x})$
+ Calculus rules:
  + $f\left(x_{1}, x_{2}\right)=g\left(x_{1}\right)+h\left(x_{2}\right) \quad f^{*}\left(y_{1}, y_{2}\right)=g^{*}\left(y_{1}\right)+h^{*}\left(y_{2}\right)$
  + $f(x)=\alpha g(x) \quad f^{*}(y)=\alpha g^{*}(y / \alpha)$, or $(g \alpha)^{*}=\alpha g^{*}$
  + $f(x)=g(x)+a^{T} x+b \quad f^{*}(y)=g^{*}(y-a)-b$
  + $f(x)=g(x-b) \quad f^{*}(y)=b^{T} y+g^{*}(y)$
  + $f(x)=g(A x) \quad f^{*}(y)=g^{*}\left(A^{-T} y\right)$
  + $f(x)=\inf _{u+v=x}(g(u)+h(v)) \quad f^{*}(y)=g^{*}(y)+h^{*}(y)$
### Examples
To find the conjugate:
+ $f^*(y) = \max_x(y^Tx-f(x))$
+ $\nabla_x (y^Tx-f(x)) = 0$ to find $x^*$
+ Put back $f^*(y) =y^Tx^*-f(x^*)$

$$
\begin{array}{l|l}
f(x) & f^{*}(y) \\
\hline \frac{1}{2} x^{T} Q x & \frac{1}{2} y^{T} Q^{-1} y \\
\hline I_{G}(x)=\{0 \text { if } x \in G, \infty \text { otherwise }\} & \max _{x \in G^{\prime}} x^{T} y \\
\hline \text { some norm }\|x\| & \text { dual norm }\|y\|\\
\hline f(x)=\sum_{i=1}^{n} x_{i} \log x_{i} & f^{*}(y)=\sum_{i=1}^{n} e^{y_{i}-1} \\
\hline f(x)=-\sum_{i=1}^{n} \log x_{i} & f^{*}(y)=-\sum_{i=1}^{n} \log \left(-y_{i}\right)-n \\
\hline f(X)=-\log \operatorname{det} X \quad\left(\operatorname{dom} f=\mathbf{S}_{++}^{n}\right) & f^{*}(Y)=-\log \operatorname{det}(-Y)-n \\
\end{array}
$$

Dual Norm: $\|z\|_{*}=\sup \left\{z^{T} x \mid\|x\| \leq 1\right\} = \sup _{x \neq 0} \frac{z^{T} x}{\|x\|}$

### Applications
#### Duality

1.  $\min _{x} f(x)+g(x)$
    $\min _{x, z} f(x)+g(z) \text { s.t. } x=z$
    $L(x, z, \mu)=f(x)+g(z)+\mu^{T}(z-x)$
    $$\begin{aligned}
    \operatorname{dual}(\mu) &=\min _{x, 2} L(x, z, \mu) \\
    &=\min _{x}\left\{f(x)-\mu^{T} x\right\}+\min _{z}\left\{g(z)+\mu^{T} z\right\} \\
    &=-\left[\max _{x}\left\{\mu^{T} x-f(x)\right\}\right]-\left[\max _{z}\left\{-\mu^{T} z-g(z)\right\}\right] \\
    &=-f^{*}(\mu)-g^{*}(-\mu)
    \end{aligned}$$

2. 
$$\begin{array}{c}
\min _{x}\{f(x)+g(A x)\} \\
\min _{x, z} \max _{\mu}\left\{f(x)+g(z)+\mu^{T}(A x-z)\right\} \\
\max _{\mu}\left\{-f^{*}\left(A^{T} \mu\right)-g^{*}(-\mu)\right\}
\end{array}$$

3. 
$$
\begin{array}{c}
\min _{x}\|y-A x\|_{2}^{2}+\lambda\|x\|_{1} \\
\text { where }\|y-A x\|_{2}^{2}=g(A x) \text { and } \lambda\|x\|_{1}=f(x) \\
\min _{\mu}\|y-\mu\|_{2}^{2} \text { s.t. }\left\|A^{T} \mu\right\|_{\infty} \leq \lambda
\end{array}
$$

#### Decoupled objective
1. 
$$
\begin{aligned}
\min _{x} & \sum_{i} f_{i}\left(x_{i}\right) \text { s.t. } a^{T} x=b \\
g(\mu) &=\min _{x} \sum_{i} f_{i}\left(x_{i}\right)+\mu\left(b-A^{T} x\right) \\
&=\mu b+\sum_{i} \min _{x_{i}}\left\{f_{i}\left(x_{i}\right)-\mu a_{i} x_{i}\right\} \\
&=\mu b-\sum_{i=1}^{n} f_{i}^{*}\left(a_{i} \mu\right)
\end{aligned}
$$
$\max _{x}\left\{\mu b-\sum_{i} f_{i}^{*}\left(a_{i} \mu\right)\right\}$ is over a concave function, easy computation.
$x_{i}^{*}=\arg \min _{x_{i}} f_{i}\left(x_{i}\right)-a_{i} \mu^{*} x_{i}$

#### Dual Ascent
+ Dual problem is always convex
![](https://cdn.mathpix.com/snip/images/OYbBztNrNImYNV6M_oy2QijkwrLeD2yp5DhrDVaSmAs.original.fullsize.png)

## Week 9,10 Gradient Descent
### Motivation
+ For unconstrained problem, $\nabla f(x^*) =0$ gives optimal.
+ Oracle access method: we do not know $f$, only access via query
  + First order oracle: access to $\nabla f(x)$, 
    + $x_{t+1} = x_t + \eta d$, 
    + $d=-\nabla f(x)$
### Gradient Descent
+ $x_{k+1}=x_{k}-\eta_{k} \nabla f\left(x_{k}\right)$, optimal when $\nabla =0$
+ choose stepsize:
  + Constant
  + Diminishing $\eta_{k} \rightarrow 0, \sum_{k} \eta_{k}=\infty$
  + Line search (exact / backtracking)

### Convergence
+ For $x_{k+1} = g(k) x_0$, convergence = $\log (g(k))$.i.e.:
  + $f(x)=a x^{2}, x_{0}=1$ -> $g(k)=(1-2a\eta )^k$, linear convergence
### Smoothness: Lipschitz (less convex than a quadratic)
$f$ has $M$ -Lipschitz gradients if
$$
\|\nabla f(x)-\nabla f(y)\| \leq M\|x-y\| \forall x, y
$$
#### Properties
+ Smoothness of function, self-tuning: get arbitrarily close to optimal solution without very small $\eta$
+ $g(x)=\frac{M}{2} x^{\top} x-f(x)$ is convex
  + $\nabla^{2} g(x)=M I-Q \succeq 0$, $M$ is the biggest eigen value of $Q$[Question: Q have to be PSD?]
+ $f$ is less convex than $\frac{M}{2} x^{T} x$
+ $f(y) \leq f(x)+\nabla f(x)^{T}(y-x)+\frac{M}{2}\|y-x\|_{2}^{2}$ Quadratic upper bound
+ for $x_{+}=x-\eta \nabla f(x)$, 
  + $f\left(x_{+}\right) \leq f(x)-\eta\|\nabla f(x)\|^{2}+\frac{M}{2} \eta^{2}\|\nabla f(x)\|^{2}$, 
  + we have descent condition: $\eta<\frac{2}{M} \Rightarrow f\left(x_{+}\right)<f(x)$
  + Steepest descent condition: $\eta=\frac{1}{M}$
  + Convergence rate for $\eta=\frac{1}{M}$: 
      + $f\left(x_{T}\right)-f^* \leq \frac{1}{T} \frac{1}{2 \eta}\left\|x_{0}-x^{*}\right\|^{2}$, i.e. $f\left(x_{T}\right)-f^* \leq O\left(\frac{M}{T}\right)$, i.e. convergence rate = $O(\frac{1}{T})$
+ For a fixed step size $t$, convergence condition is $\nabla^{2} f \npreceq(2 / t) I$
#### Examples:
+ $f(x)=a x^{2}$ -> $M=2a$

### Strongly convex (more convex than a quadratic)
+ Motivation: for very flat convex functions, convergence to optima $x^*$, rather than $f(x^*)$ can be very slow when getting close to optima.

$f$ is $m$ -strongly convex if and only if
$$
\forall x, y\langle\nabla f(x)-\nabla f(y), x-y\rangle \geq m\|x-y\|^{2}
$$
#### Properties
+ GD fast progress when far away ($\eta f(x)$ is big when far from $f(x^*)$)
+ $f(y) \geq f(x)-\frac{1}{2 m}\|\nabla f(x)\|^{2}$
+ $\text { If } \nabla^{2} f(x) \text { exists, then } \nabla^{2} f(x) \geq m I$ i.e. minimum eigenvaue of hessian $\ge m$
+ $g(x)=f(x)-\frac{m}{2} x^{\top} x$ is convex
  + $\nabla^{2} g(x)=Q-m I \succeq 0$, $m$ is the smallest eigen value of $Q$
+ $f(y) \geq f(x)+\nabla f(x)^{T}(y-x)+\frac{m}{2}\|x-y\|^{2}$

### M-Lipshitz and m-strongly convex
![](@attachment/Clipboard_2020-11-05-14-46-46.png)

#### Convergence rate at $\eta=\frac{1}{M}$:
$$m \leq \frac{\|\nabla f(x)-\nabla f(y)\|}{\|x-y\|} \leq M$$
$$
\begin{aligned}
f\left(x_{+}\right) & \leq f(x)+\langle\nabla f(x),-\eta \nabla f(x)\rangle+\frac{M}{2}\|\eta \nabla f(x)\|^{2} & [M-lipschitz]\\
&=f(x)-\eta\left[1-\frac{\eta M}{2}\right]\|\eta \nabla f(x)\|^{2} \\
&=f(x)-\frac{1}{2 M}\|\eta \nabla f(x)\|^{2} & [\eta=\frac{1}{M}]\\
& f\left(x_{+}\right) \leq f(x)+\frac{m}{M}(f(x *)-f(x)) & [m-Strongly\; convex]\\
& f\left(x_{+}\right)-f(x^*) \leq\left[1-\frac{m}{M}\right](f(x *)-f(x)) &[-f(x^*) on both sides]
\end{aligned}
$$
+ If both smooth and strongly convex - linear convergence: $f(x_T)-f(x^*) \le O((1-m/M)^T)$ 
+ If only smooth - sublinear convergence: $f(x_T)-f(x^*) \le O(1/T)$
+ If only strongly convex: may not be smooth or differentiabe, can diverge

### Line Search
#### Motivation
+ In practice $M$ is unknown
#### Exact Line Search
$$
\eta_{t}=\arg \min _{\eta} f\left(x_{t}-\eta d_{t}\right)
$$
$d=p_k$ is the descent direction: $\left\langle\mathbf{p}_{k}, \nabla f\left(\mathbf{x}_{k}\right)\right\rangle<0$
+ $f\left(x_{t}-\eta d_{t}\right)$ is a convex function of $\eta,$ and $x_{t+1}=x_{t}-\eta d_{t}$
+ Correctness: when $f$ is convex, $f(x-\eta d) \geq f(x)-\eta \nabla f(x)^{T} d$,   $\eta \nabla f(x)^{T} d$ is the biggest descrease possible
+ Problem: computationally expensive

#### Backtracking Line Search

backtracking line search (with parameters $\alpha \in(0,1 / 2), \beta \in(0,1))$, choose step size $t$ small enough to get a fraction of largest possible decrease while not using a very small $t$.
$$
\begin{array}{l}
B T L S(\alpha, \beta): \\
\qquad \begin{array}{l}
\text { until } f(x-\eta d)<f(x)-\alpha \eta \nabla f(x)^{T} d \\
\eta \leftarrow \eta \beta
\end{array} \\
\text { Here, } \alpha<\frac{1}{2} \text { and } \beta<1
\end{array}
$$
+ If $f$ is M-smooth convex, then $\eta_{B T L S} \geq \frac{\beta}{M}$ and $f\left(x_{+}\right) \leq f(x)-\frac{\alpha \beta}{M}\|\nabla f(x)\|^{2}$

![](@attachment/Clipboard_2020-11-05-15-21-35.png)

#### Compare theoretical and BTLS
+ M known:
  + $\eta = 1/M$
  + $f(x_T)-f^*\le \frac{M}{2T}\|x_0-x^*\|^2$
  + with m-strong convexity, $(1-\frac{m}{M})$ factor decrease each step
+ BTLS
  + $\eta \ge \frac{\beta}{M}$
  + $f(x_T)-f^*\le \frac{M}{2T\alpha \beta}\|x_0-x^*\|^2$
  + with m-strong convexity, $(1-min(2m\alpha , \frac{2\alpha \beta m}{M}))$ factor decrease each step

## Week 11 - Gradient Descent Methods
### Projected Gradient Descent
+ $x_{t+1}=\operatorname{Proj}_{x}\left(x_{t}-\eta \nabla f\left(x_{t}\right)\right)$
  + Proj: $\Pi_{X}(y)=\underset{x \in X}{\operatorname{argmin}} \frac{1}{2}\|x-y\|_{2}^{2}$
+ Key step: Projection
  + Project point onto plane: 
    + Take the displacement vector from the point in the plane to the given point: $\mathbf{v}=(x-d, y-e, z-f)$
    + $\mathbf{w}$ normal vector to the plane
    + $\mathbf{v}_{\perp}=\mathbf{v}-\frac{\mathbf{v} \cdot \mathbf{w}}{\|\mathbf{w}\|^{2}} \mathbf{w}$
    + projection is $(d, e, f)+\mathbf{v}_{\perp}$
  + Projection point onto norm ball: 
    + L2: $x = \arg \min _{\|\mathbf{x}\|_{2}^{2} \leq 1}\|\mathbf{x}-\mathbf{y}\|_{2}^{2}=\frac{\mathbf{y}}{\max \left\{1,\|\mathbf{y}\|_{2}^{2}\right\}}$
  + Generally, projection are quadratic programming: 
$$
\begin{array}{ll}
\operatorname{minimize} & \sum_{i=1}^{n}\left(x_{i}-y_{i}\right)^{2} \\
\text { subject to } & y \geq 0 \\
& \|y\|_p=1
\end{array}
$$
+ Properties of projection
  1. If $y \in X,$ then $\Pi_{X}(y)=y$
  2. The projection onto a convex set $X$ is non-expansive (contracting map). $\left\|\Pi_{X}(x)-\Pi_{X}(y)\right\|_{2} \leq\|x-y\|_{2}$
  $\left\|\Pi_{X}(x)-\Pi_{X}(y)\right\|_{2}^{2} \leq\left\langle\Pi_{X}(x)-\Pi_{X}(y), x-y\right\rangle \leq\|x-y\|_{2}^{2}$
  3. Optimal solution is fixed point $x^{*}=\Pi_{X}\left(x^{*}-\frac{\nabla f\left(x^{*}\right)}{L}\right)$
  4. ==Not affine invariant==
+ Convergence rate: Unconstrained case, convergence rate is the same 
  + f smooth: $O(1/T)$, i.e. $f\left(x_{t+1}\right)-f\left(x_{t}\right) \leq-\frac{\left\|\nabla f\left(x_{t}\right)\right\|^{2}}{2 M}$
  + f smooth and strongly convex: Linear $O[(1-m / M)^T]$, i.e. $\langle\nabla f(x)-\nabla f(y), x-y\rangle \geq \frac{m M}{m+M}\|x-y\|^{2}+\frac{1}{m+M}\|\nabla f(x)-\nabla f(y)\|^{2}$
  + allowed step size: $\eta=1 / M$
+ Trajectory: ==move perpendicular to level sets and then along boundary==
+ Issue: projection may be slow for some $C$

### Frank Wolfe Method
$$
\begin{aligned} s_{k-1}  \in \arg \min _{s \in C} \nabla f\left(x_{k-1}\right)^{T} s & [s_{k-1} \text{is always an extreme point, linear program in } s]\\ 
x_{k} =\left(1-\gamma_{k}\right) x_{k-1}+\gamma_{k} s_{k-1} & [\text{update to convex combination, always feasible}]\\
\gamma_{k}=\frac{2}{k+1} & [\text{step size decreases when getting close}]
\end{aligned}
$$
+ Key step: Solve argmin:
  + For $\|x\| \leq 1$ constraints
$$
\begin{aligned}
s & \in \arg \min _{\|s\| \leq 1} \nabla f\left(x_{k-1}\right)^{T} s \\
& \in-\partial\left\|\nabla f\left(x_{k-1}\right)\right\|_{*}
\end{aligned}
$$
+ Properties:
  + Works well for polytopes constraints - solve a sequence of linear programs
  + No projection step, fast in some cases
  + involves information of constraints
  + affine invariant
+ Convergence rate: similar, in general $O(1/k)$ sublinear, i.e. $f\left(x^{(k)}\right)-f^{*} \leq \frac{2 M}{k+2}$
+ Trajectory: ==move towards extreme point==

### Coordinate Descent
At each time $t,$ pick a coordinate $i$ and compute
$$
\begin{aligned}
x_{i}^{(t+1)} & \leftarrow \arg \min _{x_{i}} f\left(x_{i}, x_{j\ne i}^{(t)}\right) \\
x_{j}^{(t+1)} & \leftarrow x_{j}^{(t)} \forall j \neq i
\end{aligned}
$$
The choice of $i$: cyclic (most popular), random, greedy, etc.

+ Properties
  + If $f$ is smooth, then $x$ is optimal
    + Proof: If $\nabla f(x)$ exists,$f\left(x+\delta e_{i}\right) \geq f(x) \forall e_{i}, \forall \delta \Leftrightarrow \nabla f(x)=0$, thus $x$ is a 0 -derivative point.
  + If not smooth, generally not necessarily hold, but hold for intermediate case: $f$ can be written as ==smooth function plus a separable non-smooth function.==
$$
\min _{x} g(x)+\sum_{i=1}^{d} h_{i}\left(x_{i}\right)
$$
  + Not necessarily faster than GD because ==1 cycle of CD ~ 1 step of GD==
  + For equality constraints that is not separable, pick two coordinates and optimize jointly to satisfy the constraint
+ Key: separable functions
  + L1 norm = $\sum_{i}\left|x_{i}\right|$
+ Example: 
$$
\begin{array}{c}
\min _{x} \frac{1}{2}\|y-A x\|_{2}^{2}+\lambda\|x\|_{1} \\
0=A_{i}^{T} A_{i} x_{i}+A_{i}^{T}\left(A_{\backslash i} x_{\backslash i}-y\right)+\lambda s_{i} \\
s_i = sign(x_i)\\
x_{i}=sorh_{\frac{\lambda}{\left\|A_{i}\right\|^{2}}}\left(\frac{A_{i}^{T}\left(y-A_{\backslash i} x_{\backslash i}\right)}{\left\|A_{i}\right\|^{2}}\right)\\
\end{array}
$$

Soft threshold $sorh$: 
$$
(x)_{+}=\{\begin{array}{lcc}
x & \text { if } & x \geq 0 \\
0 & \text { otherwise }
\end{array}
$$

