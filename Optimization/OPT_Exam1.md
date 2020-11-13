---
attachments: [Clipboard_2020-11-09-16-49-03.png]
tags: [Optimization]
title: OPT Exam 1
created: '2020-11-10T00:02:59.650Z'
modified: '2020-11-10T16:48:21.402Z'
---

# OPT Exam 1
## Linear Algebra Basics
+ Norms
  + $\|\mathbf{x}\|_{\infty}:=\max _{i}\left|x_{i}\right|$
  + $\|\boldsymbol{x}\|_{1}:=\sum_{i=1}^{n}\left|x_{i}\right|$
  + $\|\boldsymbol{x}\|:=\sqrt{\boldsymbol{x} \cdot \boldsymbol{x}}$
+ Cauchy-schwaz inequality
  + $|\langle\mathbf{u}, \mathbf{v}\rangle|^{2} \leq\langle\mathbf{u}, \mathbf{u}\rangle \cdot\langle\mathbf{v}, \mathbf{v}\rangle$
  + $|\langle\mathbf{u}, \mathbf{v}\rangle| \leq\|\mathbf{u}\|\|\mathbf{v}\|$
+ Hölder's inequality
  + $p, q \in$ $[1, \infty)$ with $1 / p+1 / q=1$: $\|f g\|_{1} \leq\|f\|_{p}\|g\|_{q}$
+ Half-space: $H_{s \cdot b}^{+}=\left\{x \in \mathbb{R}^{n}: s^{\top} x \geqslant b y\right\}$

## Convex sets
### Definition
 $x_{1}, x_{2} \in C, \quad 0 \leq \theta \leq 1 \quad \Longrightarrow \quad \theta x_{1}+(1-\theta) x_{2} \in C$
### Operation that preserves convexity
- intersection
- affine functions
  - scaling, translation, rotation, projection
  - solution set of linear matrix inequality $\left\{x \mid x_{1} A_{1}+\cdots+x_{m} A_{m} \preceq B\right\}$ $\left(\right.$ with $\left.A_{i}, B \in \mathbf{S}^{p}\right)$
  - hyperbolic cone $\left\{x \mid x^{T} P x \leq\left(c^{T} x\right)^{2}, c^{T} x \geq 0\right\}$ (with $P \in \mathbf{S}_{+}^{n}$ )
- perspective function $P(x, t)=x / t$, $\mathbf{R}^{n+1} \rightarrow \mathbf{R}^{n}$
- linear-fractional functions $f(x)=\frac{A x+b}{c^{T} x+d}$

### Examples of Convex Sets
+ Convex hull: 
$$\begin{array}{l}
x=\theta_{1} x_{1}+\theta_{2} x_{2}+\ldots+\theta_{n} x_{n} \\
\text { for } \theta_{1}+\theta_{2}+\ldots+\theta_{n}=1, \theta_{i} \geq 0
\end{array}$$
+ Affine combination of $x_{1}, x_{2}, \ldots, x_{n}$ $x=\theta_{1} x_{1}+\theta_{2} x_{2}+\ldots+\theta_{n} x_{n}$
+ Hyperplanes $\left\{x \mid a_{T} x=b\right\}$
+ Halfspaces $\left\{x \mid a^{T} x \leq b\right\}$
+ Conic combination $x=\theta_{1} x_{1}+\theta_{2} x_{2}+\ldots+\theta_{n} x_{n},$ all $\theta_{i} \geq 0$
+ Ellipses $(x-C)^{T} M(x-C) \leq 1$
where $M$ is positive semidefinite or $\left\{\left.x|\| A x-b|\right|_{2} \leq 1\right\}$
+ polyhedron
+ the set of all positive definite matrices
+ Norm balls: $\{x:\|x\| \leq u\}$
+ Polynomial lower bounded on a set
+ Not convex: 
  + Norm balls with $p<1$
  + Unions of convex sets
  + Polynomial whose max on a set is lower bounded
### Separating Hyperplane theorem
$A$ and $B$ two disjoint nonempty convex subsets of $\mathrm{R}^{n}$. Then there exist a nonzero vector $v$ and
a real number $c$ such that
$$
\langle x, v\rangle \geq c \text { and }\langle y, v\rangle \leq c
$$
for all $x$ in $A$ and $y$ in $B ;$ i.e., the hyperplane $\langle\cdot, v\rangle=c, v$ the normal
vector, separates $A$ and $B$.

## Convex Functions
### Definition
+ $f\left(\lambda x_{1}+(1-\lambda) x_{2}\right) \leq \lambda f\left(x_{1}\right)+(1-\lambda) f\left(x_{2}\right)$ for $\lambda \in[0,1]$
+ $f(y) \geq f(x)+\nabla f(x)^{T}(y-x)$
+ $\nabla^{2} f(x) \geq 0$
+ $\forall x, y(\nabla f(x)-\nabla f(y))^{T}(x-y) \geq 0$
### Examples of Convex Functions
+ linear $a^{T} x+b$
+ Exponential $e^{a^{T} x}$
+ polynomial powers $\geq 1, \quad x^{2}, x^{3}, x^{1.5}$
+ negative entropy $x \log x$
+ p-norms for $p \geq 1\left(\left|x_{1}\right|^{p}+\ldots+\left|x_{d}\right|^{p}\right)^{\frac{1}{p}}$
+ spectral norm (maximum singular value of $X$ )
+ log det $X$
+ log sum exponential

### Operations Preserving Convex Functions
+ $a_{1} f_{1}(x)+a_{2} f_{2}(x)$: $a_{1}, a_{2} \geq 0$
+ $g(x)=f\left(a^{T} x+b\right)$
+ $g(x)=\min _{y} f(x, y)$

## Convex program
### Definition
+ objective function is convex + constraint set is convex
+ Standard definition: objective function & inequality constraints are convex function, equality constraints are affine
+ Standard form
$$
\begin{array}{c} 
\min _{x} f_{0}(x) & \text{ [convex] }\\
f_{i}(x) \leq 0, i=1, \ldots, m & \text{ [convex] }\\
A x=b & \text{ [affine] }
\end{array}
$$

### Properties
+ global optima = local optima
+ Any level set $L_{C}=\{x: f(x) \leq c\}$ is convex [note: set of $x$ (domain), not range of $f$]

## Linear Programs
### Definitions
+ Decoupled notation
$$
\begin{array}{l}
\quad \min _{x 1 . \ldots \times 2} \sum_{j=1}^{d} c_{j} x_{j} \\
\text { s.t. } \sum_{j=1}^{d} a_{i j} x_{j} \leq b_{i} \text { for } i=1 \ldots m \\
\sum_{j} d_{i j} x_{j}=f_{i} \text { for } i=1 \ldots p
\end{array}
$$
+ Vector notation
$$
\min _{x} c^{T} x
$$
s.t. $A x\le b$ 
$C x=d$ 
where $a_{i j}$ is the $(i, j)^{t h}$ element of $\mathrm{A}$

### Properties
+ Extreme point: 
  + Definition 1 : it is not the convex combination of any two other points in the polytope If $\exists y, z \in P, \lambda \in[0,1]$ such that $x=\lambda y+(1-\lambda) z,$ then $x$ is not extreme.
  + Definition 2 : it is the unique optimum for some cost vector $c$ $c^{T} x<c^{T} y$ for any $y \in P$
  + Equivalent: Basic feasible solution (BFS): $x$ is a BFS if its active set $\mathcal{A}_{x}$ has $n$ linearly independent vectors.
  + If an LP has a finite optimum, and its constraint polytope has at least one extreme point, then there is an extreme point which is optimal.
### Formulating LP
#### Convert to standard form [$\min. c^Tx, \quad Ax=b, x\ge 0$]

+ $$
  x\in \mathbb{R}^n, s.t. \;A_1x\le b_1, A_2x=b_2 \Rightarrow\\
  x_i = x_i^+-x_i^-, \quad \text{ where } x_i^+,x_i^- \ge 0\\
  \min. c^Tx+0^Ts, \; s\ge0\\
  A_1x+Is=b_1\\
  A_2x+0s=b_2\\
  c^{\prime}=\left[\begin{array}{c}
  c \\
  -c\\
  0
  \end{array}\right];\quad
  A^{\prime}=\left[\begin{array}{c}
  A_1 & -A_1 & I \\
  A_2 & -A_2 & I
  \end{array}\right];\quad
  b^{\prime}=\left[\begin{array}{c}
  b_1 \\
  b_2
  \end{array}\right]
  $$

+ $$
  \min. |x_1|+|x_2|, \quad s.t.\quad Ax\le b, \text{ where } x=\left[\begin{array}{c}
  x_1 \\
  x_2
  \end{array}\right] \Rightarrow\\
  \min. z_1+z_2, \text{ where } -z_1\le x_1\le z_1; \; -z_2\le x_2\le z_2\\
  \text {OR}\\
  \min(x_1^++x_1^-)+(x_2^++x_2^-), \quad s.t.\quad Ax^+-Ax^-\le b,\text{ where } x^+, x^- \ge0
  $$

#### Convert non-LP to LP

+ minimize $\|A x-b\|_{1}$ subject to $\|x\|_{\infty} \leq 1$. 
  $$
  \begin{array}{ll}
  \text {minimize} & \mathbf{1}^{T} y \\
  \text {subject to} & -y \leq A x-b \leq y \\
  & -\mathbf{1} \leq x \leq \mathbf{1}
  \end{array}
  $$

+ minimize $\|x\|_{1}$ subject to $\|A x-b\| \leq 1$. 
  $$
  \begin{array}{ll}
  \text { minimize } & \mathbf{1}^{T} y \\
  \text {subject to} & -y \leq x \leq y \\
  & -\mathbf{1} \leq A x-b \leq \mathbf{1}
  \end{array}
  $$

+ $\operatorname{minimize}\|A x-b\|_{1}+\|x\|_{\infty}$
  $$
  \begin{array}{ll}
  \text { minimize } & \mathbf{1}^{T} y+\alpha \\
  \text {subject to} & -y \leq A x-b \leq y \\
  & -\alpha \leq x \leq \alpha
  \end{array}
  $$

+ minimize $\min. \sum_{m}^{i=1} \max \left\{0, a_{i}^{T} x+b_{i}\right\}$ The variable is $x \in \mathbb{R}^{n}$
  $$
  \begin{array}{ll}
  \operatorname{minimize} & \sum_{i=1}^{m} \gamma_{i} \\
  \text {subject to} & 0 \leq \gamma_{i} \quad i=1, \ldots, m \\
  & a_{i}^{T} x+b_{i} \leq \gamma_{i} \quad i=1, \ldots, m
  \end{array}
  $$

+ minimize $\max _{\|y\|_{1}=1}\left\|\left(A_{0}+x_{1} A_{1}+\ldots+x_{p} A_{p}\right) y\right\|_{1}$

$$
\|A y\|_{1}=\left\|\sum_{i=1}^{n} a_{i} y_{i}\right\|_{1} \leq \sum_{i=1}^{n}\left|y_{i}\left\|\mid a_{i}\right\|_{1} \leq \max _{i}\left\|a_{i}\right\|_{1}\right.\\
\begin{array}{ll}
\text {minimize} & \max _{i}\left\|a_{i}\right\|_{1} \\
\text {subject to} & a_{i}=\left(A_{0}+x_{1} A_{1}+\ldots+x_{p} A_{P}\right)_{i} \quad i=1, \ldots, m,
\end{array}\\
\begin{aligned}
&\text {minimize} \quad \gamma\\
&\begin{array}{ll}
\text {subject to} & a_{i}=\left(A_{0}+x_{1} A_{1}+\ldots+x_{p} A_{P}\right)_{i} \quad i=1, \ldots, m \\
& -y_{i} \leq a_{i} \leq y_{i} \quad i=1, \ldots, m \\
& \mathbf{1}^{T} y_{i} \leq \gamma \quad i=1, \ldots, m
\end{array}
\end{aligned}
$$

+ minimize $\max _{i=1, \ldots, p}\left\|I-A_{i} X\right\|_{\infty}$

$$
\begin{array}{ll}
\text {minimize} & t \\
\text {subject to} & \left\|I-A_{i} X\right\|_{\infty} \leq t
\end{array}\\
\left[\|H\|_{\infty}=\max _{i=1, \ldots, m} \sum_{j=1}^{n}\left|H_{i j}\right| \right]\\
\begin{array}
&\text { minimize } & t\\
\text {subject to } & \sum_{k=1}^{n}\left|\left(I-A_{i} X\right)_{j k}\right| \leq t
\end{array}\\
\begin{array}{ll}
\text { minimize } \quad t\\
\text {subject to} & T_{i} \mathbf{1} \leq t \mathbf{1} \quad i=1, \ldots, p \\
& -T_{i} \leq I-A_{i} X \leq T_{i} \quad i=1, \ldots, p,
\end{array}
$$

### LP solutions

+ 

$$
\begin{array}{cl}
\operatorname{minimize} & c^{T} x \\
\text { subject to } & 0 \leq x \leq 1
\end{array}
$$
 $x_{i}=0$ when $c_{i} \geq 0,$ and $x_{i}=1$ when $c_{i} \leq 0 .$ Optimal value is $\sum_{i} \min \left(0, c_{i}\right)$

+ 

$$
\begin{array}{cl}
\operatorname{minimize} & c^{T} x \\
\text { subject to } & -1 \leq x \leq 1
\end{array}
$$
 $\sum_{i}-\left|c_{i}\right|=-\|c\|_{1} .$

+ 

$$
\begin{array}{cl}
\operatorname{minimize} & c^{T} x \\
\text { subject to } & -1 \leq 1^{T} x \leq 1
\end{array}
$$
Problem is bounded only when $c$ is parallel to 1 . $c=a \mathbf{1}$ and $c^{T} x=a \mathbf{1}^{T} x \geq-|a| .$ optimal value is $-|a|$ when $\mathbf{1}^{T} x=\operatorname{sign}(a)$

+ 

$$
\begin{array}{cl}
\operatorname{minimize} & c^{T} x \\
\text { subject to } & 1^{T} x=1, x \geq 0
\end{array}
$$

The optimal value is $\min _{i=1, \ldots, n} c_{i}$ where $x_{argmin}=1, 0 \text{ else}$

+ 

$$
\begin{array}{ll}
\text { maximize } & c^{T} x \\
\text { subject to } & 1^{T} x=k, 0 \leq x \leq 1
\end{array}
$$
optimal value is $\sum_{i=1}^{k} c_{[i]}$ where $c_{[i]}$ denotes i $^{\text {th }}$ largest value in $c$.

+ 

$$
\begin{array}{ll}
\text { maximize } & c^{T} x \\
\text { subject to } & 1^{T} x \leq k, 0 \leq x \leq 1
\end{array}
$$
do not give weights to negative $c_i$ Optimal value is $\sum_{i=1}^{k} \max \left(0, c_{[i]}\right)$

+ 

$$
\begin{array}{ll}
\text { maximize } & c^{T} x \\
\text { subject to } & d^{T} x=\alpha, 0 \leq x \leq 1
\end{array}
$$

Change variables by $x_{i} \leftarrow x_{i}^{\prime} / d_{i},$ getting
$$
\begin{aligned}
\max & \sum_{i=1}^{n}\left(c_{i} / d_{i}\right) x_{i}^{\prime} \\
\text {subject to} & \sum_{i=1}^{n} x_{i}^{\prime}=\alpha \\
& 0 \leq x_{i}^{\prime} \leq d_{i}
\end{aligned}
$$
That is, sort $\frac{c_{[i]}}{d_{[i]}}$ in descending order, assign $x_{[i]}^{\prime}=d_{[i]}$ from $i=1$ to $k,$ where $k$ is the largest index that satisfies $\sum_{i=1}^{k} d_{[i]} \leq \alpha .$ Then set $x_{[i+1]}^{\prime}=\alpha-\sum_{i=1}^{k} d_{[i]}$ and 0 for remained $x_{[i]}$

+ 

$$
\begin{array} [ll]
\text{ minimize } & c^{T} x \\
\text { subject to } & 0 \leq x_{1} \leq x_{2} \leq \ldots \leq x_{n} \leq 1
\end{array}
$$
==Optimal value is $\min _{j}\left(\sum_{i=j}^{n} c_{i}\right)$.==

+ 

$$
\begin{array}{ll}
\text { maximize } & c^{T} x \\
\text { subject to } & -y \leq x \leq y, 1^{T} y=k, y \leq 1
\end{array}
$$
 rewrite the problem as
$$
\begin{array}{ll}
\text {maximize} & \sum_{i=1}^{n}\left|c_{i} y_{i}\right| \\
\text {subject to} & \mathbf{1}^{T} y=k \\
& 0 \leq y \leq \mathbf{1}
\end{array}
$$
optimal value $\sum_{i=1}^{k}|c|_{[i]}$ where $|c|_{[i]}$ is the $i^{\text {th}}$ largest value among $\left|c_{i}\right|$.

+ 

$$
\begin{array}{cl}
\operatorname{minimize} & 1^{T} u+1^{T} v \\
\text { subject to } & u-v=c, u \geq 0, v \geq 0
\end{array}
$$
The variables are $u \in \mathbb{R}^{n}$ and $v \in \mathbb{R}^{n}$ Solution The minimum possible value of $u_{i}+v_{i}$ is $\left|c_{i}\right|$ when $u_{i}-v_{i}=c_{i}$ with $u_{i}, v_{i} \geq 0$ To see that, suppose $c_{i} \geq 0 .$ Then, $u_{i}+v_{i}=c_{i}+2 v_{i} \geq c_{i}$ where equality is attained when $v_{i}=0 .$ Otherwise, $u_{i}+v_{i}=-c_{i}+2 u_{i} \geq-c_{i}$ where similarly equality is attained when $u_{i}=0 .$ Therefore, optimal value is $\sum_{i=1}^{n}\left|c_{i}\right|$

+ 

$$
\begin{array}{cl}
\operatorname{minimize} & d_{1}^{T} u-d_{2}^{T} v \\
\text { subject to } & u-v=c, u \geq 0, v \geq 0
\end{array}
$$
The variables are $u \in \mathbb{R}^{n}$ and $v \in \mathbb{R}^{n} .$ We assume that $d_{1} \geq d_{2}$. Solution We follow the similar procedure. Suppose $c_{i} \geq 0 .$ Then, $d_{1, i} u_{i}-d_{2, i} v_{i}=$ $\left(d_{1, i}-d_{2, i}\right) v_{i}+d_{1, i} c_{i} \geq d_{1, i} c_{i} .$ Otherwise, $d_{1, i} u_{i}-d_{2, i} v_{i}=\left(d_{1, i}-d_{2, i}\right) u_{i}+d_{2, i} c_{i} \geq d_{2, i} c_{i}$
In turn, we have $d_{1, i} u_{i}-d_{2, i} v_{i} \geq d_{1, i} c_{i} 1_{\left\{c_{i} \geq 0\right\}}+d_{2, i} c_{i} 1_{\left\{c_{i} \leq 0\right\}} .$ Thus, the optimal solution
$i s$
$$
\sum_{i=1}^{n}\left(d_{1, i} 1_{\left\{c_{i} \geq 0\right\}}+d_{2, i} 1_{\left\{c_{i} \leq 0\right\}}\right) c_{i}
$$
where sgn is the sign function which gives +1 when $c_{i} \geq 0$ and -1 when $c_{i} \leq 0$

## Dual

### Process of writing
+ $\min _{x} c^{\prime} x$, $\text { s.t. } A x=b \quad Dx\ge f \text { and } x \geq 0$
+ $\min _{x \geq 0}\max _{p,q} \ c^{\prime} x+p^{\prime}(b-A x) + q(f-Dx)$: terms in max should be <= 0
+ $\max \min (c'x -p'A-qD)x+p'b+qf$: terms in min should be >= 0
+ $\max pb+qf,\; q\ge 0, \; c'-pA-qD\ge 0$
### Properties
+ Dual of dual is primal
### Duality
#### Weak duality
$\min _{x}\left\{\max _{y} f(x, y)\right\} \geq \max _{y}\left\{\min _{x} f(x, y)\right\} \Rightarrow c^{\prime} \times(\text { primal value }) \geq p^{\prime} b \text { (dual value) }$
#### Strong duality
+ Farkas' lemma:  For any vectors $c$ and $a_{i}, i \in I$ either 
  + (a)
  $\exists$ some $p_{i} \geq 0$ s.t.
  $$
  c=\sum_{i} a_{i} p_{i}
  $$
  + or (b)
  $\exists d$ s.t. $d^{\prime} a_{i} \geq 0$ for all $i,$ but $d^{\prime} c<0$
+ Corollary to Farkas' lemma:
  + If vectors $c$ and $a_{i}, i \in I$ are such that the following holds :
  + For any $d$ satisfying $d^{\prime} a_{i} \geq 0$ it has to be that $d^{\prime} c \geq 0$ Then, $\exists p_{i} \geq 0$ s.t. $c=\sum p_{i} a_{i}$
+ Strong duality: primal optimal = dual optimal

#### Complementary slackness
$x, y$ are optimal iff:
$(b_i -\sum_j a_{ij}x_j)y_i=0\; \forall i$
$(\sum_i a_{ij}y_j -c_j)x_j=0\; \forall j$
dual/primal constraints/variable cannot be $\ne$ at the same time

### Max-Flow Min-Cut Duality
#### Max-Flow
$$
\max _{f \geq 0} f_{t s}
$$
s.t. $f_{u v} \leq c_{u v}$ for all $(u, v)$ and $\sum_{v} f_{v u}=\sum_{v} f_{u v}$ for all $u$

#### Min-cut
$$
\min _{d, p} \sum_{u, v} c_{u v} d_{u v}
$$
s.t. $\forall d_{u v}, d_{u v} \geq p_{u}-p_{v}$
$p_{s}-p_{t} \geq 1$
$d_{u v} \geq 0$

### Robust Linear Programming
$$\min_x c^Tx, \; a_i^Tx\le b_i\; \forall i$$
+ Type 1: $c \in \mathcal{U}_{c}$
  + Reduce to type 2: $\min _{x, \alpha} \alpha$, $\text { s.t. } c^{T} x \leq \alpha, c \in \mathcal{U}_{c}$
+ Type 2: $\text { s.t. } a_{i}^{T} x \leq b_{i} \forall i, \text { where } a_{i} \in \mathcal{U}_{a_{i}}$, if $a_i$ is constrained within polytope, can be reduced:
  + Polytope: $\mathcal{U}_{a_{i}}=\left\{a_{i} \mid D_{i} a_{i} \leq d_{i}\right\}$
  + Subproblem is not LP (a*x): $\text { s.t. }\left\{\max _{a_{i}} a_{i}^{T} x \text { s.t. } a_{i} \in \mathcal{U}_{\alpha_{i}}, D_{i} a_{i} \leq d_{i}\right\} \leq b_{i}$
  + Dual of subproblem to separate variables into LP: $\left\{\min _{p_{i}} p_{i}^{\prime} d_{i}, p_{i} \geq 0, D_{i}^{T} p_{i}=x\right\} \leq b_{i}$
  + Put subproblem back to main problem: $\forall i, p_{i}^{T} d \leq b_{i}, D_{i}^{T} p_{i}=x, p_{i} \geq 0$
+ Type 3: $b_{i} \in \mathcal{U}_{b_{i}}$
  + Reduce to type 2: $a_{i}^{T} x \leq b_{i}^{\min }$

