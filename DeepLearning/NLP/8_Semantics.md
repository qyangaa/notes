## Semantics modeling

### Basics

+ Key idea: can interpret natural language expressions as set-theoretic expressions called models of those sentences
+ We can now concretely define things like entailment: A entails B if in all worlds where $\mathrm{A}$ is true, $\mathrm{B}$ is true
+ Powerful logic formalism including things like entities, relations, and quantification
  + Lady Gaga sings
    + sings is a predicate (with one argument), function $\mathrm{f}$ : entity $\rightarrow$ true/false
    + sings(Lady Gaga) = true or false, have to execute this against some database (world)
    + [[sings]] = denotation, set of entities which sing (found by executing this predicate on the world - we'll come back to this)

#### Quantification

+ Universal quantification: "forall" operator
  + $\forall x \operatorname{sings}(x) \vee$ dances $(x) \rightarrow$ performs $(x)$
    "Everyone who sings or dances performs"
    Existential quantification: "there exists" operator
  + $\exists x \operatorname{sings}(x) \quad$ "Someone sings"
    Source of ambiguity! "Everyone is friends with someone"
    $\forall x  \exists$ y friend $(x, y)$
    $\exists y \forall x$ friend $(x, y)$

#### Logic

+ Question answering:
  Who are all the American singers named Amy?
  $\lambda x$. nationality $(x, US A) \wedge \operatorname{sings}(x) \wedge$ first Name $(x,$ Amy $)$
  + Function that maps from $x$ to true/false, like filter. Execute this on the world to answer the question
    Lambda calculus: powerful system for expressing these functions
+ Information extraction: $\quad$ 
  + Lady Gaga and Eminem are both musicians
    musician(Lady Gaga) $\wedge$ musician(Eminem)
    Can now do reasoning. Maybe know: $\forall x$ musician $(x)=>$ performer $(x)$
    $\text { Then: performer(Lady Gaga) } \wedge \text { performer(Eminem) }$

### Montague Semantics

+ Database entities: 

Id $\quad$ Name $\quad$ Alias $\quad$ Birthdate Sings?
e4 70 Stefani Germanotta Lady Gaga $\quad 3 / 28 / 1986 \quad \mathrm{~T}$
e728 Marshall Mathers Eminem $10 / 17 / 1972$

+ Denotation$[[\text { Lady } G a g a]]=\text { e470 } \quad [[\text { sings }(e 470)]]=\text { True } $

Examples:

<img src="/home/arkyyang/files/notes/notes/attachments/image-20210309123324904.png" alt="image-20210309123324904" style="zoom:67%;" />![image-20210309123336240](/home/arkyyang/files/notes/notes/attachments/image-20210309123336240.png)

<img src="/home/arkyyang/files/notes/notes/attachments/image-20210309123405066.png" alt="image-20210309123405066" style="zoom:60%;" />

<img src="/home/arkyyang/files/notes/notes/attachments/image-20210309123450812.png" alt="image-20210309123450812" style="zoom:67%;" />

+ Challenges

<img src="/home/arkyyang/files/notes/notes/attachments/image-20210309123518482.png" alt="image-20210309123518482" style="zoom:67%;" />

### Semantic Parsing

+ Syntactic parsing: right structure
+ Semantic Parsing: meaning
+ Applications: database querying/question answer: produce lambdacalculus expressions that can be executed in these contexts

#### Combinatory Categorical Grammar

+ Syntactic categories (for this class): S, NP, "slash" categories
  SINP: "if I combine with an NP on my left side, I form a sentence" - verb

![image-20210309123654925](/home/arkyyang/files/notes/notes/attachments/image-20210309123654925.png)

![image-20210309123744224](/home/arkyyang/files/notes/notes/attachments/image-20210309123744224.png)

+ Challenge: derivation of logical form is hard
+ GENLEX: takes sentence $S$ and logical form L. Break up logical form into chunks
  $\mathrm{C}(\mathrm{L})$, assume any substring of $\mathrm{S}$ might map to any chunk
+ Latent variable model learns how to map input to derivations through the learning process, supervised only by the correct answer

## Seq2seq model

### Basics

+ Learn semantic mapping directly

$\text { What states border Texas } \longrightarrow \lambda \mathrm{x} \text { state }(\mathrm{x}) \wedge \text { borders }(\mathrm{x}, \mathrm{e} 89)$

+ Syntactic parsing

$\text { The dog ran } \longrightarrow \text { (S (NP (DT the) (NN dog) ) (VP (VBD ran) ) ) }$

+ Generating next word:
  + $\text { W size is | vocab| } \times \text { |hidden state } \mid \text { , softmax over entire vocabulary }$
  + $P(\mathbf{y} \mid \mathbf{x})=\prod_{i=1}^{n} P\left(y_{i} \mid \mathbf{x}, y_{1}, \ldots, y_{i-1}\right)$
  + $P\left(y_{i} \mid \mathbf{x}, y_{1}, \ldots, y_{i-1}\right)=\operatorname{softmax}(W \bar{h})$

![image-20210309124107346](/home/arkyyang/files/notes/notes/attachments/image-20210309124107346.png)

+ Generating sequence: feed prediction to next RNN state, until STOP is reached

![image-20210309124239389](/home/arkyyang/files/notes/notes/attachments/image-20210309124239389.png)

### Implementation

![image-20210309124529283](/home/arkyyang/files/notes/notes/attachments/image-20210309124529283.png)

+ $\text { Objective: maximize } \sum_{(\mathbf{x}, \mathbf{y})} \sum_{i=1}^{n} \log P\left(y_{i}^{*} \mid \mathbf{x}, y_{1}^{*}, \ldots, y_{i-1}^{*}\right)$
+ Encoder:  (Other architectures than RNN: CNN, Transformer)
  
+ consumes sequence of tokens, produces a vector. Analogous to RNN encoders for classification/tagging tasks
  
+ Decoder: 

  + separate module, single cell. Two inputs: hidden state (vector $h$ or tuple $(h, c))$ and previous token. Outputs token $+$ new state
  + Decoder: execute one step of computation at a time, so computation graph is formulated as taking one input + hidden state
    + Test time: do this until you generate the stop token
    + Training: do this until you reach the gold stopping point

+ Training:

  + Teacher forcing: One loss term for each target-sentence word, feed the correct word regardless of model's prediction
  + Scheduled sampling : with probability $p$, take the gold as input, else take the model's prediction
    Starting with $p=1$ (teacher forcing) and decaying it works best
  + Reinforcement learning

+ Batching: label: [num timesteps x batch size x num labels]

+ Beam search: $\operatorname{argmax}_{\mathbf{y}} \prod_{i=1}^{n} P\left(y_{i} \mid \mathbf{x}, y_{1}, \ldots, y_{i-1}\right)$

  ![image-20210309124909509](/home/arkyyang/files/notes/notes/attachments/image-20210309124909509.png)

+ Data augmentation: encode invariance to entity ID

  + ![image-20210309125307257](/home/arkyyang/files/notes/notes/attachments/image-20210309125307257.png)

### Applications

#### Semantic Parsing as Translation

+ Benefits: no grammar engineering, latent derivations, ...
+ Concerns: may produce invalid logical forms...and does it work?

#### Regex Generation

![image-20210309125357853](/home/arkyyang/files/notes/notes/attachments/image-20210309125357853.png)

+ Problem: requires a lot of data: 10,000 examples needed to get $\sim 60 \%$ accuracy on pretty simple regexes

+ Does not scale when regex specifications are more abstract (/ want to recognize a decimal number less than 20$)$

#### SQL generation

+ Convert natural language description into a SQL query against some $\mathrm{DB}$
+ How to ensure that wellformed SQL is generated?
  Three seq2seq models: Aggregation classifier, SELECT column pointer, WHERE clause pointer decoder
+ How to capture column names + constants?
  Pointer mechanisms



### Problem with Seq2seq Models

+ Like to repeat themselves: 
  + Models trained poorly
    LSTM state is not behaving as expected so it gets stuck in a "loop" of generating the same output tokens again and again
  + Need some notion of input coverage or what input words we've translated
+ Scales poorly

+ Problem with unknown words



## Attention

![image-20210309130103032](/home/arkyyang/files/notes/notes/attachments/image-20210309130103032.png)

### Basics

+ At each decoder state, compute a distribution over source inputs based on current decoder state, use that in output layer

$$
\begin{array}{l}
\text{attention function: } e_{i j}=f\left(\bar{h}_{i}, h_{j}\right)\\
\text{attention score: }\alpha_{i j}=\frac{\operatorname{cxp}\left(c_{i j}\right)}{\sum_{j^{\prime}} \exp \left(e_{i j^{\prime}}\right)} \\
\text{Weighted sum of input state: } c_{i}=\sum_{j} \alpha_{i j} h_{j} \\
\text{Concat to compute: }
P\left(y_{i} \mid \mathbf{x}, y_{1}, \ldots, y_{i-1}\right)=\operatorname{softmax}\left(W\left[c_{i} ; \bar{h}_{i}\right]\right)\\

\end{array}
$$

+ Attention function variants:
  + $f\left(\bar{h}_{i}, h_{j}\right)=\tanh \left(W\left[\bar{h}_{i}, h_{j}\right]\right)$
    Bahdanau+ (2014): additive
  + $f\left(\bar{h}_{i}, h_{j}\right)=\bar{h}_{i} \cdot h_{j}$
    Luong $+(2015):$ dot product
  + $f\left(\bar{h}_{i}, h_{j}\right)=\bar{h}_{i}^{\top} W h_{j}$
    Luong $+(2015):$ bilinear
  + Can compute after RNN cell (use hidden output), or before (use cell state)

+ Adavntage
  + LSTMs can learn to count - gives position-based addressing
  + LSTMs can learn to reject certain inputs - allows us to ignore undesirable inputs from attention
  + Encoder hidden states capture contextual source word identity
  + Decoder hidden states are now mostly responsible for selecting what to attend to
  + Doesn't take a complex hidden state to walk monotonically through a sentence and spit out word-by-word translations

### Training

![image-20210309130458020](/home/arkyyang/files/notes/notes/attachments/image-20210309130458020.png)

+ Pointer networks

  + standard decoder: softmax over vocabulary, all words get $>0$ prob

  + Pointer network: predict from source words instead of target vocab
    $$
    P_{\text {pointer }}\left(y_{i} \mid \mathbf{x}, y_{1}, \ldots, y_{i-1}\right) \propto\left\{\begin{array}{l}
    h_{j}^{\top} V \bar{h}_{i} \text { if } y_{i}=w_{j} \\
    0 \text { otherwise }
    \end{array}\right.
    $$

  + <img src="/home/arkyyang/files/notes/notes/attachments/image-20210322190925134.png" alt="image-20210322190925134" style="zoom:67%;" />

+ Pointer Generator Mixture

  + $$
    P\left(y_{i} \mid \mathbf{x}, y_{1}, \ldots, y_{i-1}\right)=P(\text { copy }) P_{\text {pointer }}+(1-P(\text { copy })) P_{\text {vocab }}
    $$

#### Handle Unknown words: add ability to copy words

$$
P\left(y_{i} \mid \mathbf{x}, y_{1}, \ldots, y_{i-1}\right)=\operatorname{softmax}\left(W\left[c_{i} ; \bar{h}_{i}\right]\right)\\
c_i: \text{ from attention}\\
h_i: \text{ from hidden state}
$$

+ Copy from source
+ or use subword tokens

##### determine subword tokens

+ Byte Pair Encoding (BPE):
  + Count bigram character cooccurrences in dictionary
  + Merge the most frequent pair of adjacent characters
+ Word Pieces (SentencePiece by Google)



#### Sentence Encoders

+ LSTM: context-aware
+ CNN: filters achieve similar function
+ Problem: LSTM and CNN don't do self-attention over long distance

### Self-attention

+ Each word forms a "query" which then computes attention over each word

+ $$
  \begin{array}{l}
  \alpha_{i, j}=\operatorname{softmax}\left(x_{i}^{\top} x_{j}\right) \quad \text { scalar } \\
  x_{i}^{\prime}=\sum_{j=1}^{n} \alpha_{i, j} x_{j} \quad \text { vector = sum of scalar * vector }
  \end{array}
  $$

+ Multiple "heads" analogous to different convolutional filters. Parameters $W_{k}$ and $V_{k}$ give different attention values (multiple head because softmax can only point to single peak)
  $$
  \alpha_{k, i, j}=\operatorname{softmax}\left(x_{i}^{\top} W_{k} x_{j}\right) \quad x_{k, i}^{\prime}=\sum_{j=1}^{n} \alpha_{k, i, j} V_{k} x_{j}
  $$
  <img src="/home/arkyyang/files/notes/notes/attachments/image-20210322191743525.png" alt="image-20210322191743525" style="zoom:67%;" />

### Transformers

+ Built on self-attention layers + feedforward layers

![image-20210322192138398](/home/arkyyang/files/notes/notes/attachments/image-20210322192138398.png)

+ Augment word embedding with position embeddings, each dim is a sine/cosine wave of a different frequency. Closer points = higher dot products
+ 