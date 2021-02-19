## Part-of-speech Tagging

+ Tags:

|      | Open class     |            | Closed class |             |
| ---- | -------------- | ---------- | ------------ | ----------- |
|      | Nouns (proper) | IBM, Italy | Determiners  | the, some   |
|      | Nouns (common) | Cat, snow  | Conjunctions | and, or     |
|      | Verbs          | see, tweet | Pronouns     | I, she, you |
|      | Adj            | beautiful  | Auxiliaries  | had [Verb]  |
|      | Adv            | swiftly    | modals       | could       |
|      |                |            | Prepositions | up, in, to  |
|      |                |            | Particles    | [make] up   |

+ Example

| Fed               | raises       | interest | rates | 0.5  | percent |
| ----------------- | ------------ | -------- | ----- | ---- | ------- |
| NNP (proper noun) | NNS (plural) | NN       | NNS   | CD   | NN      |
| VBN (normal)      | VBZ          | VBP      | VBZ   |      |         |
| VBD (derived)     |              | VB       |       |      |         |

### Setup

+ Input: $\bar{x}=\left(x_{1}, x_{2}, \ldots, x_{n}\right)$
+ Output, tag at each word: $\bar{y}=\left(y_{1}, y_{2}, \cdots, y_{n}\right)$



### Naive approach: Sequence Tagging with Classifiers

+ Option 1: predict each $y_i$ independently with logistic regression
  $$
  P\left(y_{i}=y \mid \bar{x},{i}\right)\\
  f(\bar{x}, y=N N)=\\
  \left[000 f(x) 000\left|\frac{100100}{N N}\right| 0000 \cdots\right]
  $$

+ Option 2: positional, single feature vector on current word
  $$
  f(\bar{x}, y=N N, i=3)=\\
  \left[\begin{array}{lll}
  0 & - & \mid \begin{array}{c}
  0010000 \\
  \text { interest, NN }
  \end{array} \mid& 0
  \end{array}\right]
  $$

+ Option 3: positional features with context

  + Use indicator:
    $$
    \text { Indicator [curr word = interest } \and \operatorname{tag}=N N \text { ] }
    $$
    

![image-20210214154340498](/home/arkyyang/files/notes/notes/attachments/image-20210214154340498.png)

+ Problem with classification for tagging:
  + predictions may be incoherent, many combination works but are not correct

## Hidden Markov Model

### Setup

+ Tags: $y_{i} \in \tau_{\text {tags }}$
+ Words: $x_{i} \in \underset{\text { vocabulary }}{V}$
+ Objective: $P(\bar{y}, \bar{x})=P\left(y_{1}\right) P\left(x_{1} \mid y_{1}\right) P\left(y_{2} \mid y_{1}\right) P\left(x_{2} \mid y_{2}\right) \cdots P\left(\text{STOP} \mid y_{n}\right)$

+ Markov process: $y_i$ conditionally dependent  only on $y_{i-1}$

+ Parameters

  + Initial distribution: $S = \R^\tau$
    + $S[i]$: probability of i'th tag to be at the first position
  + Transitions: $T = \R^{\tau \times \tau}$
    + $T[i][j]$: transition from i'th tag to j'th tag
  + Emissions: $T=\R^{\tau \times V}$
    + $P(x_k|y_k) = P[i][j]$: $x_k = \text{words}[j]$, $y_k = \text{tags}[i]$

  ![image-20210214155404681](/home/arkyyang/files/notes/notes/attachments/image-20210214155404681.png)

### Parameter Estimation

+ Maximize log likelihood:
  $$
  \sum_{i} \log P(\bar{y}^{(i)}, \bar{x}^{(i)}) = \\
  =\sum_i \log P(y_{i}^{(i)})+\sum_{i} \sum_{j} \log P(x_{j}^{(i)} \mid y_{j}^{(i)})+\sum_{i} \sum_{j} \log P(y_{j}^{(i)} | y_{j-1}^{(i)}) \\
  = \text{S + E + T}
  $$

+ MLE can be estimated by count + normalization (cross row, cross column separately):

  + Eg: HHHT, $3 / 4=\frac{\text{argmax}}{p} 3 \log p+\log (1-p)$

  + $$
    \tau=\{N, V, S T O P\} \quad V=\text \{ they, can, fish \}\\
    $$

    + Data: 

      | N    | V    | STOP |
      | ---- | ---- | ---- |
      | they | can  |      |
      | N    | V    | STOP |
      | they | fish |      |

    + ![image-20210214160410516](/home/arkyyang/files/notes/notes/attachments/image-20210214160410516.png)

+ Smoothing: add 1 to all cells and then normalize

#### Example:

| P(y_1): (S) | N    | V    |
| :---------- | :--- | :--- |
|             | 1    | 0    |

| P(y_i+1 \| y_i): (T) | N    | V    | STOP |
| :------------------- | :--- | :--- | :--- |
| y_i = N              | 1/5  | 3/5  | 1/5  |
| y_i = V              | 1/5  | 1/5  | 3/5  |

| P(x_i \| y_i): (E) | they | can  | fish |
| :----------------- | :--- | :--- | :--- |
| y_i = N            | 1    | 0    | 0    |
| y_i = V            | 0    | 1/2  | 1/2  |

Sequence:

| N    | V    | V    |
| ---- | ---- | ---- |
| they | can  | fish |

+ Probability of sequence:
  + $P(N)P(they|N)P(V|N)P(can|V)P(V|V)P(fish|V)P(STOP|V) =\\ 1*1*3/5*1/2*1/5*1/2*3/5$



### Inference

$$
\operatorname{argmax}_{\bar{y}} P(\bar{y} \mid \bar{x})=\operatorname{argmax}_{\bar{y}} \frac{P(\bar{y}, \bar{x})}{P(\bar{x})} =\operatorname{argmax}_{\bar{y}} P(\bar{y}, \bar{x})=\operatorname{argmax}_{\bar{y}} \log P(\bar{y}, \bar{x})\\
=\underset{\tilde{y}_{1}, \cdots, \tilde{y}_{n}}{\operatorname{argmax}} \log P\left(\tilde{y}_{1}\right)+\log P\left(x_{1} \mid \tilde{y}_{1}\right)+\log P\left(\tilde{y}_{2} \mid \tilde{y}_{1}\right)+\ldots
$$

#### Viterbi Dynamic Program

+ Define $v_{i}(\tilde{y})= \R^{n \times \tau}$ the best path ending in each tag among all tags at position $i$

+ Base case: $v_{1}(\tilde{y})=\log P\left(x_{1} \mid \tilde{y}\right)+\log P(\tilde{y})$

+ Recursion: $v_{i}(\tilde{y})=\log P\left(x_{i} \mid \tilde{y}\right)+\text{argmax}_{y_{prev}} [\log P(\tilde{y}|y_{prev}) + v_{i-1}(y_{prev})]$

  +  = Emission + argmax(transition + prev)

+ Algorithm:

  ```python
  for i in range(1,n+1):
      for y in tau:
          v[i][y] = ...
  return max(v[n+1][STOP])
  ```

+ Find path by tracking back pointers

+ Example

| log P(y_1): | N    | V    |
| :---------- | :--- | :--- |
|             | -1   | -1   |

| log P(y_i+1 \| y_i) | N    | V    | STOP |
| :------------------ | :--- | :--- | :--- |
| y_i = N             | -2   | -1   | -1   |
| y_i = V             | -1   | -1   | -2   |

| log P(x_i \| y_i): | they | can  | fish |
| :----------------- | :--- | :--- | :--- |
| y_i = N            | -1   | -3   | -1   |
| y_i = V            | -3   | -1   | -1   |

|      | they | can                     | fish                     | STOP             |
| :--- | :--- | :---------------------- | :----------------------- | ---------------- |
| N    | -2   | -7                      | (-7): -7 + -2 + -1 = -10 |                  |
|      |      |                         | (-4):  -4 + -1 + -1 = -6 | (-7): -6+-1 = -7 |
| V    | -4   | (-2): -2 + -1 + -1 = -4 | (-7): -7 + -1 + -1 = -9  |                  |
|      |      | (-4): -4 + -1 + -1 = -6 | (-4):  -4 + -1 + -1 = -6 | (-6): -6+-2 = -8 |

+ Time complexity:  $O\left(n|\tau|^{2}\right)$ because each cell looks back at $\tau$ cells

#### Beam Search

+ Instead of looking back at $\tau$ cells, only look at $k$ top choices from last index (with a priority queue)
+ Run time: $O(n k|\tau| \log k)$
+ k = 2~100

<img src="/home/arkyyang/files/notes/notes/attachments/image-20210214170858834.png" alt="image-20210214170858834" style="zoom:67%;" />



### State of the art

#### Trigram taggers

1. Normal HMM "bigram" model: $\mathrm{y}_{1}=\mathrm{NNP}, \mathrm{y}_{2}=\mathrm{VBZ}, \ldots$
2. Trigram model: $y_{1}=(<S>, N N P), y_{2}=(N N P, V B Z), \ldots$
3. Probabilities now look like $\mathrm{P}((\mathrm{NNP}, \mathrm{VBZ}) \mid(<\mathrm{S}>, \mathrm{NNP}))-$ more context! We know the verb is occurring two words after $<\mathrm{S}>$
4. Tradeoff between model capacity and data size - trigrams are a "sweet spot" for POS tagging

#### HMM State of the art

1. Normal HMM "bigram" model: $\mathrm{y}_{1}=\mathrm{NNP}, \mathrm{y}_{2}=\mathrm{VBZ}, \ldots$
2. Trigram model: $y_{1}=(<S>, N N P), y_{2}=(N N P, V B Z), \ldots$
3. Probabilities now look like $\mathrm{P}((\mathrm{NNP}, \mathrm{VBZ}) \mid(<\mathrm{S}>, \mathrm{NNP}))-$ more context! We know the verb is occurring two words after $<\mathrm{S}>$
4. Tradeoff between model capacity and data size - trigrams are a "sweet spot" for POS tagging

#### Challenging part

<img src="/home/arkyyang/files/notes/notes/attachments/image-20210214171111950.png" alt="image-20210214171111950" style="zoom:67%;" />

1. Lexicon gap (word not seen with that tag in training): $4.5 \%$ of errors
2. Unknown word: $4.5 \%$
   Could get right: $16 \%$ (many of these involve parsing!)
3. Difficult linguistics: $20 \%$
   VBD / VBP? (past or present?) They set **up** absurd situations, detached from reality
4. Underspecified / unclear, gold standard inconsistent / wrong: $58 \%$
   adjective or verbal participle? JJ / VBN? a $\$ 10$ million fourth-quarter charge against **discontinued** operations

#### POS with Feedforward Networks

<img src="/home/arkyyang/files/notes/notes/attachments/image-20210214171249572.png" alt="image-20210214171249572" style="zoom:67%;" />

![image-20210214171315211](/home/arkyyang/files/notes/notes/attachments/image-20210214171315211.png)

![image-20210214171338020](/home/arkyyang/files/notes/notes/attachments/image-20210214171338020.png)