### Definition

+ $T(n)$ is $O(f(n))$ if $\exists c, n_0 \rightarrow T(n)\le cf(n) \text{ for } \forall n\ge n_0$
+ $T(n)$ is $\Omega(f(n))$ if $\exists c, n_0 \rightarrow T(n)\ge cf(n) \text{ for } \forall n\ge n_0$
+ $T(n)$ is $T(f(n))$ if $T(n)$ if $O(f(n))$ and $\Omega(f(n))$



### Cases

+ Common:

|                 | Sort      | Search        | Tree     | HashMap |
| --------------- | --------- | ------------- | -------- | ------- |
| Time Complexity | $n\log n$ | $n$, $\log n$ | $\log n$ | 1       |



+ Specific

|            | Worst     | Best      | Average | Amortized |
| ---------- | --------- | --------- | ------- | --------- |
| Quick Sort | $n^2$     | $n\log n$ |         |           |
|            |           |           |         |           |
|            |           |           |         |           |
| k-sum      | $n^{k-1}$ |           |         |           |

 

### Calculation

+ `sum(basic operations * loops)`

+ Recursive deduction

  + $T(n)=T(n-1)+..=T(n-2)+T(n-1)+...$

    + |        | Merge Sort  | Binary Search | Selection Sort | Factorial |
      | ------ | ----------- | ------------- | -------------- | --------- |
      | $T(n)$ | $T(n/2)+cn$ | $T(n/2)+c$    | $T(n-1)+cn$    | $T(n-1)$  |
      | $O()$  | $n\log n$   | $\log n$      | $n^2$          | $n$       |

    

+ Solution space

+ Master Method

  + $$
    \begin{array}{l}
    T(n)=a T\left(\frac{n}{b}\right)+n^{c} \\
    \left\{\begin{array}{l}
    \text { if } \log _{b} a>c, T(n)=n^{\log _{b} a} \\
    \text { if } \log _{b} a=c, T(n)=n^{c} \operatorname{logn} \\
    \text { if } \log _{b} a<c, T(n)=n^{c}
    \end{array}\right.
    \end{array}
    $$