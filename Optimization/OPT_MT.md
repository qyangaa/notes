# MT 1

## 1

A set $X \subseteq \mathbb{R}^{n}$ that consists of a single point is: convex, by definition

## 2

the intersection of k convex sets, and 1 non-convex set: can be convex or non-convex

## 3

 Consider 10 blue points and one red point placed in 3 dimensions. Refer to all other points in R3 as black points. Let K be the set of black points that are closer to the red point, than to any of the 10 blue points.

Each of the blue points introduces a halfspace, and the results is an intersection between halfspaces - convex

## 4

Let $C$ be a convex set in $\mathrm{n}$ dimensions. Let $A$ be a $m \times n$ matrix. Consider the set $A(C),$ obtained by mapping each point of $C$ by the matrix $A$. Thus, $K=A(C)$ will be a subset of $\mathrm{m}$ dimensional space. Then:

K is always convex because:
$$
x_1, x_2 \in K \Rightarrow \exists y_1,y_2 \in C \quad s.t. \quad x_1 = Ay_1, x_2=Ay_2\\
\lambda x_1+(1-\lambda) x_2 \in K \Leftarrow y_3 = \lambda y_1+(1-\lambda)y_2
$$

## 5

Let $C$ be a NON-CONVEX set in $n$ dimensions. Let $A$ be a $m \times n$ matrix. Consider the set $A(C),$ obtained by mapping each point of $C$ by the matrix $A$. Thus, $K=A(C)$ will be a subset of $m$ dimensional space. Then: 

There is not enough information: K could be convex or non-convex

## 6

Suppose $f: \mathbb{R}^{n} \longrightarrow \mathbb{R}$ is a function of $\mathrm{n}$ variables that is continuous. Suppose $C$ is a convex subset of $\mathbb{R}^{n}$ Note that $f$ need not be convex. Consider the set $K=f(C)$. Then:

Since $f$ is continuous in $\mathbb{R}$ and C is convex, then $K$ is just an interval in $\mathbb{R}$, thus is always convex

## 7

Suppose $f: \mathbb{R}^{n} \longrightarrow \mathbb{R}$ is a function of $n$ variables that is continuous. Suppose $C$ is a convex subset of $\mathbb{R}^{n}$ Suppose that $f$ is non-convex. Let $L_{a}$ denote its sub-level set at level $a,$ i.e., $L_{a}=\{x: f(x) \leq a\}$. Then:

$L_a$ can be convex for some (non convex) or all values of $a$ (quasi convex)

## 8

Consider minimizing a convex function, $f(x),$ over a convex set $X .$ Let $X^{*}$ denote the set of optimal solutions. So if $X^{*}$ is non-empty, it means that the problem has at least one optimal solution.

$X^*$ must be convex, either infinite many points or one point

## 9

Suppose that we are minimizing a linear function $f(x)=c^{\top} x,$ over a set $X=\{x: A x \leq b\} .$ As in the previous problem, let $X^{*}$ denote the set of optimal solutions, so that $X^{*} \subseteq X$.

$X=\{x: A x \leq b\} $: polytope

+ Polytope can be unbounded: 
  + no optimal solution, $X$ non-empty, $X^*$ empty 
  + Has optimal solution on bounded side:  $X$ non-empty, $X^*$ non-empty 
+ Simply c=0: unbounded optimal solution: $X^*$ is unbounded and non-empty
+ Since set $X$ is polyhedron (can be unbounded), $X^*$ has to be polyhedron (can be single point or empty)
  + If $X^*$ is non-empty: $X^*$ is single point or non-empty polyhedron



## 10

Suppose that we are minimizing a linear function $f(x)=c^{\top} x,$ over a set $X=\{x: A x \leq b\} .$ As in the previous problem, let $X^{*}$ denote the set of optimal solutions, so that $X^{*} \subseteq X$.

+ If primal unbounded, dual infeasible - strong duality doesn't hold
+ If $X^*$ is non-empty: 
  + optimal primal objective is finite, exists primal optimal -> strong duality holds
  + $X^*$ doesn't need to be bounded

## 11

Suppose that we are minimizing a linear function $f(x)=c^{\top} x,$ over a set $X=\{x: A x \leq b\} .$ Let $p^{*}$ be a dual feasible point that is also optimal. Let $a_{i}^{\top}$ denote the inth row of $A,$ and $A_{j}$ denote the $j^{\wedge}$ th column of $A$. Consider the statements. 

I. If $p_{i}^{*}=0,$ then $a_{i}^{\top} x<b_{i} .$ Not have to be $<$ , can $=$, only the reversed way

II. If $x_{j}=0,$ then $A_{j}^{\top} p^{*}<c_{j}$ or $A_{j}^{\top} p^{*}>c_{j} .$ Only the reversed way

III. If $a_{i}^{\top} x<b_{i}$ then $p_{i}^{*}=0 .$ 

+ Dual feasible & optimal for linear program: strong duality hold ->complementary slackness hold

## 12

Suppose that we are minimizing a linear function $f(x)=c^{\top} x,$ over a set $X=\{x: A x \leq b\} .$ Let $p$ be a dual feasible point, though not necessarily optimal. Let $a_{i}^{\top}$ denote the inth row of $A,$ and $A_{j}$ denote the $j^{\wedge} t h$ column of $A$.

dual feasible

+ Dual unbounded: primal infeasible
+ Dual bounded: primal feasible bounded

## 13

The maximization of the minimum of k linear functions, is:

+ min of linear functions is concave

+ Maximization of concave function over convex set is also convex problem

+ Reformulate: 
  $$
  \max z\\
  z\le a_ix+b_i
  $$



## 14

Consider the optimization problem: $\min :\left\{c_{1} x_{1}+c_{2} x_{2}+c_{3} x_{3}: 3\left|x_{1}\right|-2\left|x_{2}\right|+0\left|x_{3}\right| \leq-1, A x \leq b\right\}$
where the term $A x \leq b$ denotes additional linear inequality constraints.

+ $-|x|\le -a \Rightarrow |x|\ge a$: feasible set is non convex set, thus cannot be made into LP

## 15

Consider the robust linear optimization problem: $\min :\left\{c^{\top} x: A x \leq b, \forall A \in U\right\},$ for $U$ a given uncertainty set. Select all that are correct.

+ if $U$ has finitely many points: then is LP
+ If $U$ is a polytope, or a union/ intersection bewteen polytope: just use a intersections of all extreme points: can be reformulated as LP (can have infinitely many points)
+ If $U$ is not the form of polytope, may not be reformulated as LP



# MT 2



## 1

Suppose $A$ is a positive semidefinite $n$ by $n$ matrix. Let $\lambda>0$ denote its largest eigenvalue. Let $x$ and $y$ be any vectors. If we say they are normalized, then we mean they are of unit Euclidean norm, i.e., $\|x\|=\|y\|=1 .$ What is the relationship between $y^{\top} A x$ and $\lambda$ ? Mark all that are true.

$y^{\top} A x \leq \lambda$, for any $x$ and $y$ that are normalized
There exists an $x$ and $y$ normalized, for which $y^{\top} A x<\lambda$
There exists an $\mathrm{x}$ and $\mathrm{y}$ normalized, for which $y^{\top} A x=\lambda$

## 2

For $M$ a symmetric matrix, let $\lambda(M)$ denote its largest eigenvalue. Suppose $A$ and $B$ are positive semidefinite matrices. Then:

Largest eigen value is a convex function

$\lambda(0.5 *(A+B)) \leq 0.5 \lambda(A)+0.5 \lambda(B)$



## 3

As above, let $\lambda(M)$ denote the largest eigenvalue of any symmetric matrix. Suppose $\mathrm{A}$ and $\mathrm{B}$ are both positive semidefinite, and $\lambda(A) \geq \lambda(B)$. Then

A is neither positive nor negative semidefinite to B

## 4

If $M_{1}$ and $M_{2}$ are symmetric $n \times n$ matrices, with all non-positive eigenvalue, then any convex combination must also have all non-positive eigenvalues.

The set of negative (positive semidefinite) matrices is convex (closed). Convex combination is also non-positive

## 5

If $M_{1}$ and $M_{2}$ are symmetric $n \times n$ matrices, each with at least one non-negative eigenvalue, then any convex combination must also have at least one non-negative eigenvalues: False



## 6,7

Consider the problem: $\min :\left\{x_{1}^{2}+x_{2}^{2}-2 x_{1}-4 x_{2}-9: x_{1}+x_{2} \leq 2, \frac{1}{3} x_{1}+x_{2} \leq 1, x_{1}, x_{2} \geq 0\right\}$
Then the optimal dual solution corresponding 

+ to the primal point $\left(x_{1}, x_{2}\right)=(0,1)$ is:

Write as $(x_1-1)^2+(x_2-2)^2-14$

For dual to be optimal:

$x_1+x_2<2$: $\lambda_1=0$

$\frac{1}{3}x_1+x_2=1$: $\lambda_2\ge0$

Stationarity

$\nabla f(x) + \lambda_1(1,1)+\lambda_2(1/3, 1)=(0,0)$ Has no solution

+ to the primal point $\left(x_{1}, x_{2}\right)=(3/2,1/2)$ is:

From Stationarity:

$(\lambda_1, \lambda_2)= (-3,6)$

$\lambda_1<0$: not feasible



## 8

Consider the function $f\left(x_{1}, x_{2}\right)=x_{1}^{2}-3 x_{1}+4\left|x_{1}\right|+2 x_{2}^{2}+x_{2}+2\left|x_{2}\right|-5 .$ Is $(0,0)^{\top}$ an element of the subdifferential at $\left(x_{1}, x_{2}\right)=(0,0) ?$



## 9

Let $f: \mathbb{R}^{n} \longrightarrow \mathbb{R}$ be a convex function, and let $g \in \partial f(x)$. Assume further that $x$ is a suboptimal point. Then $f(x-\eta g) \leq f(x)$

Not guaranteed for any choice of $\eta$ because subgradient descent is not a decent method



## 10

Let $f: \mathbb{R}^{n} \longrightarrow \mathbb{R}$ be a convex function. Suppose we start from some $x_{0}$ in the domain of $f,$ and we use the subgradient method: $x_{t+1}=x_{t}-\eta_{t} g_{t},$ where as usual, $g_{t} \in \partial f\left(x_{t}\right)$ denotes a subgradient at $x_{t} .$ Suppose our step size is not constant, and is of the form: $\eta_{t}=\frac{\eta}{t}$ for some constant $\eta$. Then, $f\left(x_{t}\right) \rightarrow f\left(x^{*}\right)$ as long as: $\eta$ is any positive value, because it is divided by $t$

## 11

Let $f: \mathbb{R}^{n} \longrightarrow \mathbb{R}$ be a $\beta$ -smooth and convex function. Then, with constant stepsize $\eta=1 / \beta,$ we have that $\left\|x_{t}-x^{*}\right\| \leq \epsilon,$ might converge very slowly unless have strong convexity

