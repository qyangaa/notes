## Calculus

+ Norms
  + $\|\mathbf{x}\|_{\infty}:=\max _{i}\left|x_{i}\right|$
  + $\|\boldsymbol{x}\|_{1}:=\sum_{i=1}^{n}\left|x_{i}\right|$
  + $\|\boldsymbol{x}\|:=\sqrt{\boldsymbol{x} \cdot \boldsymbol{x}}$
+ Cauchy-schwaz inequality
  + $|\langle\mathbf{u}, \mathbf{v}\rangle|^{2} \leq\langle\mathbf{u}, \mathbf{u}\rangle \cdot\langle\mathbf{v}, \mathbf{v}\rangle$
  + $|\langle\mathbf{u}, \mathbf{v}\rangle| \leq\|\mathbf{u}\|\|\mathbf{v}\|$
+ Hölder's inequality
  + $p, q \in$ $[1, \infty)$ with $1 / p+1 / q=1$: $\|f g\|_{1} \leq\|f\|_{p}\|g\|_{q}$

### Linear Transformation

+ Change or coordinates $A \boldsymbol{y}=\boldsymbol{x}$: calculate coordinates in new system by scaling vectors in old system by $A^{-1}$: $v' = A^{-1}v$, eg:
  $$
  v = \{(3,0),(-3,0),(0,1),(0,-1)\}\\
  A=\left[\begin{array}{ll}
  3 & 0 \\
  0 & 1
  \end{array}\right]\\
  \Rightarrow\\
  v' = A^{-1}v = \{(1,0),(-1,0),(0,1),(0,-1)\}\\
  $$

+ Find change of coordinate that turn ellipse to sphere:
  $$
  \text{Ellipse: } \quad \mathcal{E}=\left\{\boldsymbol{x}=\left(\begin{array}{l}
  x_{1} \\
  x_{2}
  \end{array}\right): \boldsymbol{x}^{\top} Q \boldsymbol{x} \leq 1\right\}\\
  \boldsymbol{y}^{\top} A^{\top} Q A \boldsymbol{y} \leq 1\\
  A = Q^{-\frac{1}{2}}=\frac{1}{\sqrt{\lambda_{1}}} \boldsymbol{v}_{1} \boldsymbol{v}_{1}^{\top}+\frac{1}{\sqrt{\lambda_{2}}} \boldsymbol{v}_{2} \boldsymbol{v}_{2}^{\top}
  $$
  



## Geometry

+ Half-space: $H_{s \cdot b}^{+}=\left\{x \in \mathbb{R}^{n}: s^{\top} x \geqslant b y\right\}$

## Special Properties

+ $Q^{-1}$ is symmetric if $Q$ is symmetric