## Machine Translation

### Introduction

+ Data: Input: sentence / Output: sentence

+ Challenge:
  + Ambiguity / many-to-many translations
  + Phrase-level correspondence

#### Framework

+ Bernard Vauquois:

![image-20210322165647397](/home/arkyyang/files/notes/notes/attachments/image-20210322165647397.png)

+ Big chunks make translation easier:
  + how to get chunks?
  + how to translate?

#### Evaluation

+ At word/ phrase level

+ BLEU: 
  + geometric mean of 1, 2, 3, 4-gram precision of the output times a brevity penalty
  + precision: how many n-grams in translation are in reference
  + property: normally low, correlates to human judgements

### Word Alignment

![image-20210322165932215](/home/arkyyang/files/notes/notes/attachments/image-20210322165932215.png)

+ $\bar{t}$: target words
+ $\bar{s}$: source words
+ $\bar{a}$: alignments, $a_i = j$: target word i align with source word j
  <img src="/home/arkyyang/files/notes/notes/attachments/image-20210322170123564.png" alt="image-20210322170123564" style="zoom:67%;" />

#### **IBM Model**

+ Probability is generative, unsupervised (no label for alignment, only two sentences)
+ NULL in source words bag  as placeholder for unaligned words

$$
P_{\theta}(\bar{t}, \bar{a}| \bar{s}) \\
\bar{a}=\left(a_{1}, \ldots, a_{n}\right) \quad \bar{t}=\left(t_{1}, \ldots, t_{n}\right) \quad \bar{s}=\left(s_{1}, \ldots, s_{m}, NULL\right)\\
P\left(\bar{t},\left.\bar{a}\right| {\bar{s}}\right)=P(\bar{a}) P(\bar{t}|\bar{a}, \bar{s})=\prod_{i=1}^{n} P\left(a_{i}\right) P\left(t_{i} \mid s_{a_{i}}\right)\\
P\left(a_{i}\right)=\text { uniform over }\{1, \ldots, m, NULL\}\\
P\left(t_{i} \mid s_{a_{i}}\right): \text{model parameter, prob of } t_i \text{ given source word } s_{a_i} ,\left|V_{s}\right| \times\left|V_{t}\right|
$$

+ Inference
  $$
  \begin{array}{l}\\
  P(\bar{a} \mid \bar{s}, \bar{t})=\frac{P(\bar{a}, \bar{t} \mid \bar{s})}{P(\bar{t} \mid \bar{s})}=\frac{\prod_{i=1}^{n}P(a_{i}) P(t_{i} \mid s_{a_{i}})}{P(t \mid s) \text { normalization constant }}\propto \prod_i P\left(t_{i} \mid s_{a_{i}}\right)   
  \end{array}\\
  P\left(a_{i} \mid \bar{s}, \bar{t}\right) \propto P\left(t_{i} \mid s_{a_{1}}\right)
  $$



+ Alignment Example

|               | I    | like | eat  |
| :------------ | :--- | :--- | :--- |
| P(* \| Je)    | 0.8  | 0.1  | 0.1  |
| P(* \| J')    | 0.8  | 0.1  | 0.1  |
| P(* \| mange) | 0    | 0    | 1    |
| P(* \| aime)  | 0    | 1    | 0    |
| P(* \| NULL)  | 0.4  | 0.3  | 0.3  |

$$
\text{I like} | \text{J' aime NULL} \\
P(a_1 |s, t) \alpha\left\{\begin{array}{lll}
0.8 & \mathrm{~J'} & 2 / 3  \\
0 & \text { aime } & 0  \\
0.4 & \mathrm{NULL} & 1 / 3 
\end{array}\right.\\
P(I|J') = 0.8/(0.8+0.4) = 2/3 \\

P(like|aime) = 1/(0.1 + 1 + 0.3) = 10/14 \\
P(\text{I like} | \text{J' aime}) =  P(I|J') * P(like|aime) = 10/21
$$

#### HMM Model

$$
P(\bar{a})=\prod_{i=1}^{n} P\left(a_{i} \mid a_{i-1}\right) =\prod_{i=1}^{n} \text { Categorical }\left(a_{i}-a_{i-1}\right)\\
$$

![image-20210322175824199](/home/arkyyang/files/notes/notes/attachments/image-20210322175824199.png)

+ Optimize: with expectation maximization (EM)
  $$
  \sum_{i=1}^{D} {P(t\mid s)} = \sum_{i=1}^{n} \log \sum_a P(\bar{t}, \bar{a} \mid \bar{s})  [\text{ marginal log likelihood}]
  $$
  





### Phase based machine translation

+ Noisy channel model

$$
\mathrm{P}(\mathbf{e} \mid \mathbf{f}) \propto \mathrm{P}(\mathbf{f} \mid \mathbf{e}) \mathrm{P}(\mathbf{e}) \\
\mathrm{P}(\mathbf{f} \mid \mathbf{e}): \text{ Translation model (TM), from phrase table}\\
\mathrm{P}(\mathbf{e}): \text{ Language model (LM)}\\
P\left(e_{i} \mid e_{1}, \ldots, e_{i-1}\right) \approx P\left(e_{i} \mid e_{i-n-1}, \ldots, e_{i-1}\right) \text{ (Beam Search)}
$$

+ Phrase lattice

![image-20210322190010761](/home/arkyyang/files/notes/notes/attachments/image-20210322190010761.png)

+ Monotonic translation

  + $$
    \text{Score: }\arg \max _{\mathbf{e}}\left[\prod_{\langle\bar{e}, \bar{f}\rangle} P(\bar{f} \mid \bar{e}) \cdot \prod_{i=1}^{|\mathbf{e}|} P\left(e_{i} \mid e_{i-1}, e_{i-2}\right)\right]\\
    \text { score = log [P(Mary) P(not | Mary) P(Maria | Mary) P(no | not)] }\\
    P(Mary) P(not | Mary): LM \\
    P(Maria | Mary) P(no | not): TM \\
    \text { In reality: score }=\alpha \log P(\mathrm{LM})+\beta \log P(T M)
    $$

  + Beam search: can look back a word or a whole phrase

  + ![image-20210322190105455](/home/arkyyang/files/notes/notes/attachments/image-20210322190105455.png)

  + Advanced:

    + More flexible model: can visit source sentence "out of order"
    + State needs to describe which words have been translated and which haven't
    - Big enough phrases already capture lots of reorderings, so this isn't as important as you think

+ Training Decoders:
  + Usually 5-20 feature weights to set, want to optimize for BLEU score which is not differentiable
  + MERT (Och 2003): decode to get 100 best translations for each sentence in
    small training set $(<1000$ sentences), line search on parameters to directly optimize for BLEU

### Syntactic Machine Translation

+ Synchronous context-free grammar as middle ground
  <img src="/home/arkyyang/files/notes/notes/attachments/image-20210322190538100.png" alt="image-20210322190538100" style="zoom:67%;" />
  + Translation = parse the input with "half" of the grammar, read off the other half
  + Assumes parallel tree structures, but there can be reordering
+ More flexibility: lexicalized rules, lead to HUGE grammars, slow parsing

![image-20210322190628293](/home/arkyyang/files/notes/notes/attachments/image-20210322190628293.png)