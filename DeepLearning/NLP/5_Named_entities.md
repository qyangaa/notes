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

+ Objective: $\text{maximize } \mathcal{L}\left(\mathbf{y}^{*}, \mathbf{x}\right)=\log P\left(\mathbf{y}^{*} \mid \mathbf{x}\right)$

+ Gradient: expectation is intractable because it sums over all possible tag sequences
  $$
  \frac{\partial}{\partial w} \mathcal{L}\left(\mathbf{y}^{*}, \mathbf{x}\right)=\sum_{i=2}^{n} f_{t}\left(y_{i-1}^{*}, y_{i}^{*}\right)+\sum_{i=1}^{n} f_{e}\left(y_{i}^{*}, i, \mathbf{x}\right)\\
  -\mathbb{E}_{\mathbf{y}}\left[\sum_{i=2}^{n} f_{t}\left(y_{i-1}, y_{i}\right)+\sum_{i=1}^{n} f_{e}\left(y_{i}, i, \mathbf{x}\right)\right]
  $$
  If not optimizing for transition:
  $$
  \frac{\partial}{\partial_{W}} \mathcal{L}\left(\mathbf{y}^{*}, \mathbf{x}\right)=\sum_{i=1}^{n} f_{e}\left(y_{i}^{*}, i, \mathbf{x}_{i}\right)-\left[\mathbb{E}_{y} \sum_{i=1}^{n} f_{e}\left(y_{i}, i, \mathbf{x}_{i}\right)\right]
  $$
  

+ Emission feature expectation:
  $$
  \begin{aligned}
  \mathbb{E}_{\mathbf{y}}\left[\sum_{i=1}^{n} f_{e}\left(y_{i}, i, \mathbf{x}\right)\right] &=\sum_{\mathbf{y} \in \mathcal{Y}} P(\mathbf{y} \mid \mathbf{x})\left[\sum_{i=1}^{n} f_{e}\left(y_{i}, i, \mathbf{x}\right)\right]\\
  & =\sum_{i=1}^{n} \sum_{\mathbf{y} \in \mathcal{Y}} P(\mathbf{y} \mid \mathbf{x}) f_{e}\left(y_{i}, i, \mathbf{x}\right) \\
  &=\sum_{i=1}^{n} \sum_{s} P\left(y_{i}=s \mid \mathbf{x}\right) f_{e}(s, i, \mathbf{x})
  \end{aligned}\\
  \text{separate to forward and backward portions: }\\
  P\left(y_{i}=s \mid \mathbf{x}\right)=\sum_{y_{1}, \ldots, y_{i-1}, y_{i+1}, \ldots, y_{n}} P(\mathbf{y} \mid \mathbf{x})
  $$

+ Transition features can be hard coded (eg. to enforce constraints for illegal transitions)

+ Algorithm:

  ```python
  for each epoch:
      for each example:
          extract features on each emission and transition (cached)
          compute potentials phi based on features + weights
          compute marginal probabilities with forward-backward
          accumulate gradient over all emsisions and transitions
  ```

#### Forward-backward algorithm

+ Objective: to compute  $P\left(y_{i}=s \mid \mathbf{x}\right)=\sum_{y_{1}, \ldots, y_{i-1}, y_{i+1}, \ldots, y_{n}} P(\mathbf{y} \mid \mathbf{x})$

+ Viterbi computes: $P\left(\mathbf{y}_{\max } \mid \mathbf{x}\right)=\max _{y_{1}, \ldots, y_{n}} P(\mathbf{y} \mid \mathbf{x})$

+ Examples: compute left and right part of two wings with viterbi separately
  <img src="/home/arkyyang/files/notes/notes/attachments/image-20210221112011551.png" alt="image-20210221112011551" style="zoom:50%;" /> <img src="/home/arkyyang/files/notes/notes/attachments/image-20210221113243558.png" alt="image-20210221113243558" style="zoom:50%;" /> ![image-20210221112130330](/home/arkyyang/files/notes/notes/attachments/image-20210221112130330.png)
  $$
  \begin{array}{l}
  P\left(y_{3}=2 \mid \mathbf{x}\right)= \\
  \quad \frac{\text { sum of all paths through state} 2 \text { at time } 3 (tiel )}{\text { sum of all paths (purple)}}
  \end{array}\\
  =\frac{\text { forward }_{i}(s) \text { backward }_{i}(s)}{\sum_{s^{\prime}} \text { forward }_{i}\left(s^{\prime}\right) \text { backward }_{i}\left(s^{\prime}\right)}\\
  P\left(s_{3}=2 \mid \mathbf{x}\right)=\frac{\alpha_{3}(2) \beta_{3}(2)}{\sum_{i} \alpha_{3}(i) \beta_{3}(i)}\\
  \text{denominator is same for all i:}\\
  {\sum_{i} \alpha_{3}(i) \beta_{3}(i) = P(\mathbf{x})}\\
  $$

+ Left (backward): 

  + Initial:
    $\alpha_{1}(s)=\exp \left(\phi_{e}(s, 1, \mathbf{x})\right)$
    $\log\alpha_{1}(s)=\phi_{e}(s, 1, \mathbf{x})$

  + Recurrence: sum instead of max (in viterbi), store as log prob since they get very small
    $\alpha_{t}\left(s_{t}\right)=\sum_{s_{t-1}} \alpha_{t-1}\left(s_{t-1}\right) \exp \left(\phi_{e}\left(s_{t}, t, \mathbf{x}\right)\right)$

    $\begin{aligned}
    \alpha_{t}^{l}\left(s_{t}\right) &=\log \left(\alpha_{t}\left(s_{t}\right)\right)\\ &=
    \log \sum_{s_{t-1}} \left(\exp \left(\alpha_{t-1}^{l}\left(s_{t-1}\right)\right)+\phi_{e}\left(s_{t}, t, \mathbf{x}\right)+\phi_{t}\left(s_{t-1}, s_{t}\right)\right)
    \end{aligned}$

+ Right (forward):
  + Initial:
    $\beta_{n}(s)=1$
  + $\log \beta_{n}(s)=0$
  + Recurrence:
    $\beta_{t}\left(s_{t}\right)=\sum_{s_{t+1}} \beta_{t+1}\left(s_{t+1}\right)\exp \left(\phi_{e}\left(s_{t+1}, t+1, \mathbf{x}\right)\right)\exp (\phi_{e}(s_{t}, s_{t+1}))$
    $\log\beta_{t}\left(s_{t}\right)=\text{logsumexp}_{s_{t+1}} [\log \beta_{t+1}\left(s_{t+1}\right)+ \left(\phi_{e}\left(s_{t+1}, t+1, \mathbf{x}\right)\right)+ (\phi_{e}(s_{t}, s_{t+1}))]$



+ Put together to find $P$:

$$
\begin{aligned}
P\left(y_{t}=s \mid \mathbf{x}\right) &=\exp \left(\log \left(\frac{\alpha_{t}(s) \beta_{t}(s)}{\sum_{i} \alpha_{t}(i) \beta_{t}(i)}\right)\right) \\
&=\exp \left(\log \left(\alpha_{t}(s)\right)+\log \left(\beta_{t}(s)\right)-\log \left(\sum_{i} \alpha_{t}(i) \beta_{t}(i)\right)\right) \\
&=\exp \left(\log \left(\alpha_{t}(s)\right)+\log \left(\beta_{t}(s)\right)-\log \left(\sum_{i} \exp \left(\log \left(\alpha_{t}(i) \beta_{t}(i)\right)\right)\right)\right) \\
&=\exp \left(\log \left(\alpha_{t}(s)\right)+\log \left(\beta_{t}(s)\right)-\log \left(\sum_{i} \exp \left(\log \left(\alpha_{t}(i)\right)+\log \left(\beta_{t}(i)\right)\right)\right)\right) \\
&=\exp \left(\alpha_{t}^{l}(s)+\beta_{t}^{l}(s)-\log \left(\sum_{i} \exp \left(\alpha_{t}^{l}(i)+\beta_{t}^{l}(i)\right)\right)\right)
\end{aligned}
$$



#### HMM vs. CRF

|      | $P\left(x_{i} \mid y_{i}\right), P\left(y_{i} \mid y_{i-1}\right)$ | $P\left(y_{i} \mid \mathbf{x}\right), P\left(y_{i-1}, y_{i} \mid \mathbf{x}\right)$ |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| HMM  | Model parameter (usually multinomial distribution)           | Inferred quantity from forward-backward                      |
| CRF  | Undefined (model is by definition conditioned on $\mathrm{x}$ ) | Inferred quantity from forward backward                      |



### NER Evaluation

+ Precision: correct / predicted
+ Recall: correct/ gold labels
+ F = 1/(1/Precision+1/Recall)



### Additional features for prediction

+ World knowledge
+ Context (non-local features)
+ Scailing up tag space