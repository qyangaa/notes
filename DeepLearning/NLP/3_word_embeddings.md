## Word Embeddings

### Motivation

+ Bag of word vector shows no hint for correlated words: `v[movie was good]*v[film is great] = [1 1 1 0 0 0]*[0 0 0 1 1 1]=0`
+ Word embeddings: low dimensional representation of words (50-300), capture similarity
+ Goal: to learn good word embeddings
+ ![image-20210202155107327](/home/arkyyang/files/notes/notes/attachments/image-20210202155107327.png)

### Brown Clusters

+ word2vec: Each word $w$ has: (hyperparameters $d$ and $k$)

  + $\bar{v}_{w}$: word vector (of dimension $d$ = 50-300)
  + $\bar{c}_{w}$: context vector, all neighbors of each word token up to $k$ positions away
  + 

+ Objective:  predict each word's context given word

+ Skip-gram: probability of context given word (among all words in vocabulary as context)
  $$
  P(\operatorname{context}=y \mid \text { word }=x)=\frac{\exp \left(\bar{v}_{x} \cdot \bar{c}_{y}\right)}{\sum_{y^{\prime} \in V} \exp \left(\bar{v}_{x} \cdot \bar{c}_{y^{\prime}}\right)}
  $$

  + $\bar{v}, \bar{c}$ are model parameters to be learned, each of dimension (num params) $2|V| * d$, where $V$ is vocabulary size

  + Skip: can skip for up to $k-1$ distance

  + Example: 

    + Corpus = I saw

    + $\bar{v}_{I}=[1,0] \quad \bar{v}_{\text {saw }}=[0,1]$

    + $\bar{c}_{\text {saw }}=[1,0] \text { and } \bar{c}_{I}=[0,1]$

    + $$
      \exp \left(\bar{v}_{\text {saw }} \cdot \bar{c}_{\text {I }}\right)\approx 3 \quad \exp \left(\bar{v}_{\text {saw }} \cdot \bar{c}_{\text {saw }}\right) = 1 \\
      P(\text { context }=I |\text { word }=\text{saw})=\frac{3}{4}\\
      P(\text { context }=\text{saw} |\text { word }=\text{saw})=\frac{1}{4}
      $$

    + ![image-20210202155133101](/home/arkyyang/files/notes/notes/attachments/image-20210202155133101.png)

+ Training:
  $$
  \begin{array}{c}
   \text { Maximize } \sum_{(x, y)} \log P(\text { context }=y \mid \text { word }=x) \\
  \text { for x,y as pairs in data }
  \end{array}
  $$

  + Impossible problem: cannot achieve P=1 due to multiple contexts existing
  + Initialize params randomly

+ Prediction:
  $$
  P\left(w^{\prime} \mid w\right)=\operatorname{softmax}(W e(w))
  $$
  ![image-20210202153139274](/home/arkyyang/files/notes/notes/attachments/image-20210202153139274.png)

  + Parameters: $d \times |V|$ word vectors, $|V| \times d$ context vectors (stacked into a matrix $W$ )

### Other Embeddings

+ Continuous bag of words: predict word from context (skip-gram: predict context from word)
  $$
  P\left(w \mid w_{-1}, w_{+1}\right)=\operatorname{softrnax}\left(W\left(c\left(w_{-1}\right)+c\left(w_{+1}\right)\right)\right)
  $$
  ![image-20210202153115080](/home/arkyyang/files/notes/notes/attachments/image-20210202153115080.png)

  + Parameters: $d \times |V|$ word vectors, $|V| \times d$ context vectors 

#### Accelerate: prevent $|V|$ matrix multiplication(expensive)

+ Hierarchical softmax: 
  ![image-20210202153428431](/home/arkyyang/files/notes/notes/attachments/image-20210202153428431.png)

+ Skip-gram with negative sampling:

  + for each (word, context) pair, create random negative examples (word, falseContext) from unigram distribution, becomes binary classification problem

  + $$
    P(y=1 \mid w, c)=\frac{e^{w \cdot c}}{e^{w \cdot c}+1}\\
    \text { Objective }=\log P(y=1 \mid w, c)+\frac{1}{k} \sum_{i=1}^{n} \log P\left(y=0 \mid w_{i}, c\right)
    $$

  + Same parameters as before

#### Connections of Skip-gram with Matrix Factorization

![image-20210202153957803](/home/arkyyang/files/notes/notes/attachments/image-20210202153957803.png)
$$
\begin{aligned}
M_{i j} &=\operatorname{PMI}\left(w_{i}, c_{j}\right)-\log k \\
\operatorname{PMI}\left(w_{i}, c_{j}\right) &=\frac{P\left(w_{i}, c_{j}\right)}{P\left(w_{i}\right)P\left(c_{j}\right)}=\frac{\frac{\operatorname{count}\left(w_{i}, c_{j}\right)}{D}}{\frac{\operatorname{count}\left(w_{i}\right)}{D} \frac{\operatorname{count}\left(c_{j}\right)}{D}}
\end{aligned}
$$

+ Skip-gram is fatoring the matrix:
  + If we sample negative examples from the unigram distribution over words
  +  it's a weighted factorization problem (weighted by word freq)

### GloVe (global vectors)

$$
\text { Objective } \left.=\sum_{i, j} f\left(\operatorname{count}\left(w_{i}, c_{j}\right)\right)\left(w_{i}^{\top} c_{j}+a_{i}+b_{j}-\log \operatorname{count}\left(w_{i}, c_{j}\right)\right)\right)^{2}
$$

+ Constant in the dataset size (just need counts), quadratic in voc size
+ By far the most common word vectors used today $(5000+$ citations $)$

### fastText: Sub-word Embeddings

+ Break words into character n-grams (n=3-6)

$$
w\cdot c \rightarrow \left(\sum_{g \in \text { ngrams }} w_{g} \cdot c\right)
$$

+ Example:
  + where:
    + 3-grams: $<$ wh, whe, her, ere, re> 
    + 4-grams: <whe, wher, here, eres>
    + 5-grams: <wher, where, heres>
    + 6-grams: <where, wheres>

### Pretrained models

+  ElMo, GPT, BERT
+ Use subwords
+ Embeddings are computed using RNNs and Transformers through language modeling. We can't just look up an embedding for each word, but actually need to run a mode

### Debiasing

+ Approach1: 
  + Identify gender subspace with gendered words
  + Project words onto this subspace
  + Subtract those projections from the original word
  + ![image-20210202152713279](/home/arkyyang/files/notes/notes/attachments/image-20210202152713279.png)
+ Not effective: bias pervades the word embedding space

