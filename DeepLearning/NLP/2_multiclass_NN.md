## Multi-class Classification

+ Problem with one-vs-all n binary classifiers: ![image-20210125131343138](/home/arkyyang/files/notes/notes/attachments/image-20210125131343138.png) likely not separable

Take following example:

```
x = too many drug trials, too few patients
classes = {health, sports, science}
f(x) = [1,1,0] (bag-of-words [drug, patients, baseball])
```

+ DW (different weights)

  + One weight vector per class: $argmax_{y\in classes} w_y^T f(x)$

  + Above example:

    ```
    w_health = [2,5.6,-3]
    w_health*f(x) = 7.6
    w_sports = [1.2, -3.1, 5.7]
    w_sport*f(x) = -1.9
    argmax = w_health
    ```

  + Better for neural network because we don't need to bring in y early on

+ DF (different features)

  + Merge class into features: $argmax_{y\in classes} w_y^T f(x,u)$

  + Above example:

    ```
    f(x,y=health) = [1,1,0,0,0,0,0,0,0] ([health, sports, science])
    f(x,y=sports) = [0,0,0,1,1,0,0,0,0]
    w = [2,5.6,-3,1.2, -3.1, 5.7,...]
    ```



### Multiclass Perceptron

```python
for t in epochs:
	for i in data:
		y_pred = argmax_y(w_y*f(x))
		if y_pred != y_i:
            # DW: 
			y_pred -= lr*f(x)
			y_i += lr*f(x)
			# DF:            
			w += lr*f(x,y_i)
```

#### Example

```python
[drug, patients, baseball]
[1,1,0] y=1: too many drug trials, too few patients (health)
[1,0,1] y=2: baseball players taking drugs (sports)
#  Assume "default" pred (break ties) y=3
w_0 = [[0,0,0], [0,0,0],[0,0,0]]
update on [1,1,0],1: (pred=3, + at 1, - at 3)
    w_1 = [[1,1,0],[0,0,0],[-1,-1,0]]
update on [1,0,1],2: (pred=1, + at 2, - at 1)
    w_2 = [[0,1,-1],[1,0,1],[-1,-1,0]]
```



### Multiclass Logistic Regression

+ Probability sums to 1:

$$
P(y=\hat{y} \mid \bar{x})=\frac{e^{\bar{w}^{\top} f(\bar{x}, \hat{y})}}{\sum_{y^{\prime} \in y} e^{\bar{w}^{\top} f(\bar{x} y^{\prime})}}
$$

+ binary logistic regression is a special case:

  + $$
    \begin{array}{l}
    y=+1: e^{\bar{w}^{\top} f(x)} \\
    y=-1: 1=e^{\bar{w}^{\top} 0}
    \end{array}
    $$

+ Gradient:
  $$
  \begin{aligned}
  \frac{\partial}{\partial \bar{w}} \operatorname{loss}\left(\bar{x}^{(i)}, y^{(i)}, \bar{w}\right) &=-f\left(\bar{x}, y^{(i)}\right)+\sum_{y^{\prime} \in y} P\left(y^{\prime} \mid \bar{x}\right) f\left(\bar{x}, y^{\prime}\right) \\
  & P\left(y^{(i)} \mid \bar{x}\right) \approx 1:-f\left(\bar{x}, y^{(i)}\right)+f\left(\bar{x}, y^{(i)}\right) \approx 0 \\
  & P\left(y_{b a d} \mid \bar{x}\right) \approx 1:-f\left(\bar{x}, y^{(i)}\right)+f\left(\bar{x}, y_{b a d}\right)
  \end{aligned}
  $$
  

+ Example

  ```python
  w_0 = [[0,0,0], [0,0,0],[0,0,0]]
  update on [1,1,0],1: lr=1
      w_1*f(x) = 0
      w_2*f(x) = 0
      w_3*f(x) = 0
      P_1 = P_2 = P_3 1/3
      w_1 -= lr*(-f(x,1)+P_1*(f(x))) = [2/3, 2/3, 0]
      w_2 -= lr*(-f(x,0)+P_2*(f(x))) = -lr*(P_2*(f(x))) = -[1/3, 1/3, 0]
      w_3 == w_2
  ```

  





## Neural Nets

### Neuron

$$
z = g(Vf(x))\\
y_{\text {pred }}=\operatorname{argmax}_{y} \mathbf{w}_{y}^{\top} \mathbf{z}\\
$$

+ $g$: activation function (nonlinear transformation)

  + `tanh; ReLU`

+ $V$: matrix (linear, warp space and shift)

+ Example:
  $$
  V=\left[\begin{array}{ll}
  1 & 0 \\
  0 & 1 \\
  1 & 1
  \end{array}\right]\\
  \bar{z}=\left[\tanh \left(x_{1}\right), \tanh \left(x_{2}\right), \tanh \left(x_{1}+x_{2}\right)\right]
  $$
  

![image-20210126130727221](/home/arkyyang/files/notes/notes/attachments/image-20210126130727221.png)

+ Softmax to obtain classification output
  $$
  \operatorname{softmax}(W f(x))\\
  \text{Each dimension (class):}\\
  \operatorname{softmax}(w_yf(x))=P(y \mid \mathbf{x})=\frac{\operatorname{cxp}\left(\mathbf{w}_{y}^{\top} f(\mathbf{x})\right)}{\sum_{y^{\prime} \in \mathcal{Y}} \exp \left(\mathbf{w}_{y^{\prime}}^{\top} f(\mathbf{x})\right)}\\
  \text {add one hidden layer:}\\
  P(\mathbf{y} \mid \mathbf{x})=\operatorname{softmax}(W g(V f(\mathbf{x})))
  $$

### Training

+ Log likelihood maximization

$$
\mathcal{L}\left(\mathbf{x}, i^{*}\right)=\log P\left(y=i^{*} \mid \mathbf{x}\right)=\log \left(\operatorname{softmax}(W \mathbf{z}) \cdot c_{i^{*}}\right)\\
=W \mathbf{z} \cdot e_{i^{*}}-\log \sum_{j} \exp (W \mathbf{z}) \cdot e_{j}
$$

### Backpropagation

![image-20210126135239813](/home/arkyyang/files/notes/notes/attachments/image-20210126135239813.png)

![image-20210126135331983](/home/arkyyang/files/notes/notes/attachments/image-20210126135331983.png)