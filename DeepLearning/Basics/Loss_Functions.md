## Classification Loss

### Hinge Loss

Multi class SVM Loss
$$
S V M L o s s=\sum_{j \neq y_{i}} \max \left(0, s_{j}-s_{y_{i}}+1\right)
$$

```python
def hingeLoss(x,correctIdx):
    sy = x[correctIdx]
    return sum([max(0,xi-sy+1) for xi in x])
```



+ Idea: correct category should be greater than sum of scores of all incorrect categories by some safety margin (usually one)
+ Application: [maximum-margin](https://link.springer.com/chapter/10.1007/978-0-387-69942-4_10) classification, most notably for [support vector machines](https://en.wikipedia.org/wiki/Support_vector_machine). 
+ Pros: Convex
+ Cons: not differentiable

### Softmax

$$
\sigma(\vec{z})_{i}=\frac{e^{z_{i}}}{\sum_{j=1}^{K} e^{z_{j}}}
$$

```python
def softmax(x):
    expSum = sum(np.exp(x))
    return np.array([np.exp(xi)/expSum for xi in x])
```

+ Idea: normalized exponentials
+ Application: classification of basic problems that are linearly separable
+ Pros: Convex, simple and fast
+ Cons: 
  + Not work if data is not linearly separable
  + Does not support null regection

### Cross Entropy

Negative Log Likelihood
$$
\text { Cross Entropy Loss }=-\sum_{i} p_{i} \log q_{i}=-y \log \hat{y}-(1-y) \log (1-\hat{y})
$$

```python
def crossEntropyLoss(p, q):
    return -sum([p[i]*log(q[i]+1e-5) for i in range(len(p))])
```



+ Idea: Penalize heavily for incorrect but confident predictions
  + correct positive : $-(1log(1-\epsilon)+0log\epsilon)\approx -1log(1)=0$
  + correct negative: $-(0log\epsilon+1log(1-\epsilon))\approx -1log(1)=0$
  + incorrect positive: $-(1log\epsilon+0log(1-\epsilon))= -1log(\epsilon)$ very large
  + incorrect negative: $-(0log(1-\epsilon)+1log\epsilon)\approx -1log(\epsilon)$ very large
+ Application:  [logistic regression](https://en.wikipedia.org/wiki/Logistic_regression),
+ <a href="https://kobejean.github.io/machine-learning/2019/07/22/cross-entropy-loss/">Pros: </a>
  + Log: numerical stability when probabilities are multiplied (joint event)
  + Well behaved gradients
+ Cons: <a href="https://openreview.net/forum?id=ByfbnsA9Km">can lead to poor margins</a>

### Softmax-Cross entropy 

Use softmax before cross entropy loss gives well behaved gradient:<a href="https://kobejean.github.io/machine-learning/2019/07/22/cross-entropy-loss/">Ref</a>
$$
f(\boldsymbol{x})=\frac{e^{x}}{\sum_{i=0}^{n} e^{x_{i}}}; \quad H(y, f(\boldsymbol{x}))=-\boldsymbol{y} \cdot \log (f(\boldsymbol{x}))\\
\begin{aligned}
\frac{\partial H(\boldsymbol{y}, f(\boldsymbol{x}))}{\partial \boldsymbol{x}} &=\frac{\partial(-\boldsymbol{y} \cdot \log (f(\boldsymbol{x})))}{\partial \boldsymbol{x}} \\
&=-\boldsymbol{y}^{\top} \frac{\partial \log \left(\frac{e^{x}}{\left.\sum_{i=0}^{n} e^{x_{i}}\right)}\right.}{\partial \boldsymbol{x}} \\
&=-\boldsymbol{y}^{\top} \frac{\partial\left(\log \left(e^{\boldsymbol{x}}\right)-\mathbf{1} \log \left(\sum_{i=1}^{n} e^{\boldsymbol{x}_{i}}\right)\right)}{\partial \boldsymbol{x}} \\
&=-\boldsymbol{y}^{\top}\left(I-\mathbf{1} \frac{\partial \log \left(\sum_{i=1}^{n} e^{\boldsymbol{x}_{i}}\right)}{\partial \boldsymbol{x}}\right) \\
&=-\boldsymbol{y}^{\top}\left(I-\mathbf{1}\left(\frac{e^{\boldsymbol{x}}}{\sum_{i=1}^{n} e^{\boldsymbol{x}_{i}}}\right)^{\top}\right) \\
&=-\boldsymbol{y}^{\top}\left(I-\mathbf{1} f(\boldsymbol{x})^{\top}\right) \\
&=\left(f(\boldsymbol{x}) \mathbf{1}^{\top}-I\right) \boldsymbol{y} \\
&=f(\boldsymbol{x}) \mathbf{1}^{\top} \boldsymbol{y}-\boldsymbol{y} \\
&=f(\boldsymbol{x}) \sum_{i=1}^{n} \boldsymbol{y}_{i}-\boldsymbol{y} \\
&=f(\boldsymbol{x})-\boldsymbol{y}
\end{aligned}
$$


### NLL Negative Log-Likelihood 

$$
L(\mathbf{y})=-\log (\mathbf{y})
$$

+ Idea: maximize y - likelihood
+ Similar to cross-entropy

### Log softmax

+ Idea: log(softmax(x))
+ Application:
+ Pro over softmax:  better numerical stability

## Regression Loss

### MSE Mean square error

quadratic/ L2 loss
$$
M S E=\frac{\sum_{i=1}^{n}\left(y_{i}-\hat{y}_{i}\right)^{2}}{n}
$$

+ Pros: Differentiable
+ Cons: Sensitive to outliers

### MAE Mean Absolute Error

L1 Loss
$$
M A E=\frac{\sum_{i=1}^{n}\left|y_{i}-\hat{y}_{i}\right|}{n}
$$

+ Pros: Simple
+ Cons: Non-differentiable, lacks stability and robustness

