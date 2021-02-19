## Named Entity Recognition (NER)

![image-20210215124148787](/home/arkyyang/files/notes/notes/attachments/image-20210215124148787.png)

+ Tages:
  + BIO: begin, inside, outside
+ Problem with HMM:
  + lot of O's
  + insufficient features/ capacity with multinomial

## Conditional random field

### Idea

+ Use arbitrary features of input
  <img src="/home/arkyyang/files/notes/notes/attachments/image-20210215124401961.png" alt="image-20210215124401961" style="zoom:67%;" />

+ 

### Setup

+ Input: full input and tag vectors: $(\mathbf{x}, \mathbf{y})$

+ Objective: probability calculated with potential functions: $P(\mathbf{y} \mid \mathbf{x})=\frac{1}{Z} \prod_{k} \exp \left(\phi_{k}(\mathbf{x}, \mathbf{y})\right)$

  + eg. linear potential: 

    + $\phi_{k}(\mathbf{x}, \mathbf{y})=w^{\top} f_{k}(\mathbf{x}, \mathbf{y})$

    + $P(\mathbf{y} \mid \mathbf{x})=\frac{1}{Z} \exp \left(\sum_{k=1}^{n} w^{\top} f_{k}(\mathbf{x}, \mathbf{y})\right)$ [TODO:]

      + $k:$
      + $n:$
      + $f:$

    + Similar to multiclass logistic regression model3
      <img src="/home/arkyyang/files/notes/notes/attachments/image-20210215125340006.png" alt="image-20210215125340006" style="zoom:50%;" />

    + Intractable: sum/ max over exponentially many $\mathbf{y}$  : 
      $$
      Z=\sum_{\mathbf{y}^{\prime}} \exp \left(\sum_{k=1}^{n} w^{\top} f_{k}\left(\mathbf{x}, \mathbf{y}^{\prime}\right)\right)\\
      \mathbf{y}_{\text {best }}=\operatorname{argmax}_{\mathbf{y}^{\prime}} \exp \left(\sum_{k=1}^{n} w^{\top} f_{k}\left(\mathbf{x}, \mathbf{y}^{\prime}\right)\right)
      $$

+ Sequential CRFs, every emission depends on the full input vector $\mathbf{x}$
  $$
  P(\mathbf{y} \mid \mathbf{x})=\frac{1}{Z} \prod_{i=2}^{n} \exp \left(\phi_{t}\left(y_{i-1}, y_{i}\right)\right) \prod_{i=1}^{n} \exp \left(\phi_{e}\left(y_{i}, i, \mathbf{x}\right)\right)\\
  \phi_t: \text{transition potential}\\
  \phi_e: \text{emission potential}
  $$
  <img src="/home/arkyyang/files/notes/notes/attachments/image-20210215184256503.png" alt="image-20210215184256503" style="zoom:67%;" />
  + Log-linear features:
    $$
    P(\mathbf{y} \mid \mathbf{x})=\frac{1}{Z} \exp w^{\top}\left[\sum_{i=2}^{n} f_{t}\left(y_{i-1}, y_{i}\right)+\sum_{i=1}^{n} f_{e}\left(y_{i}, i, \mathbf{x}\right)\right]\\
    \phi_{e}\left(y_{i}, i, \mathbf{x}\right)=w^{\top} f_{e}\left(y_{i}, i, \mathbf{x}\right) \quad \phi_{t}\left(y_{i-1}, y_{i}\right)=w^{\top} f_{t}\left(y_{i-1}, y_{i}\right)
    $$

+ Comparing HMM and CRFs:
  ![image-20210215184605417](/home/arkyyang/files/notes/notes/attachments/image-20210215184605417.png)

  | HMM                               | CRF                                 |
  | --------------------------------- | ----------------------------------- |
  | Emission consider one word a time | Emission consider full input vector |
  | Local normalization               | Global normalization                |
  | Generative                        | Discriminative                      |
  |                                   | Feature functions are flexible      |

+ Feature examples
  ![image-20210215185058362](/home/arkyyang/files/notes/notes/attachments/image-20210215185058362.png)
  + Word features (can use in HMM)
    + Capitalization
    + Word shape
    + Prefixes/suffixes
    + Lexical indicators
  + Context features (can't use in HMM!
    + Words before/after3
    + POS before/after (if we run a POS tagger first)
  - Word clusters
  + Gazetteers

### Components

#### Model

$$
\begin{array}{l}
P(\mathbf{y} \mid \mathbf{x})=\frac{1}{Z} \prod_{i=2}^{n} \exp \left(\phi_{t}\left(y_{i-1}, y_{i}\right)\right) \prod_{i=1}^{n} \exp \left(\phi_{e}\left(y_{i}, i, \mathbf{x}\right)\right) \\
P(\mathbf{y} \mid \mathbf{x}) \propto \exp w^{\top}\left[\sum_{i=2}^{n} f_{t}\left(y_{i-1}, y_{i}\right)+\sum_{i=1}^{n} f_{e}\left(y_{i}, i, \mathbf{x}\right)\right]
\end{array}
$$



#### Inference

$$
\max _{y_{1}, \ldots, y_{n}} e^{\phi_{t}\left(y_{n-1}, y_{n}\right)} e^{\phi_{e}\left(y_{n}, n, \mathbf{x}\right)} \ldots e^{\phi_{e}\left(y_{2}, 2, \mathbf{x}\right)} e^{\phi_{t}\left(y_{1}, y_{2}\right)} e^{\phi_{e}\left(y_{1}, 1, \mathbf{x}\right)}\\
\text{Similar to Viterbi:}\\
\begin{array}{l}
\log P\left(y_{i} \mid y_{i-1}\right) \rightarrow \phi_{t}\left(y_{i-1}, y_{i}\right) \\
\log P\left(x_{i} \mid y_{i}\right) \rightarrow \phi_{e}\left(y_{i}, i, \mathbf{x}\right)
\end{array}\\
\text { Maximize } \mathcal{L}\left(\mathbf{y}^{*}, \mathbf{x}\right)=\log P\left(\mathbf{y}^{*} \mid \mathbf{x}\right)
$$

+ Potentials depend on $\mathbf{x}$, which is same for all y's, does not change inference over $\mathbf{y}$

+ Compare to multiclass logistic regression:
  $$
  \text{Multiclass Logistic Regression:}\\
  P(y \mid x) \propto \exp w^{\top} f(x, y)\\
  \frac{\partial}{\partial w_{i}} \mathcal{L}\left(x_{j}, y_{j}^{*}\right)=f_{i}\left(x_{j}, y_{j}^{*}\right)-\sum_{y} f_{i}\left(x_{j}, y\right) P_{w}\left(y \mid x_{j}\right)\\
  =f_{i}\left(x_{j}, y_{j}^{*}\right)-\mathbb{E}_{y}\left[f_{i}\left(x_{j}, y\right)\right]\\
  =\text{label value} - \text{predition value}
  $$
  

#### Learning

