## Sequential models

### N-gram modeling

$$
P(\bar{w})=\prod_{i=1}^{m} p(w_{i} \mid \underbrace{\left.w_{i-n+1}, \cdots, w_{i-1}\right)}_{\text {previbus } n-1 \text { words }}\\
n \approx 3 - 7
$$

+ 3 gram: $P\left(w_{1}|\langle <s> <s>\right) P\left(w_{2} \mid\langle s\rangle w_{1}\right) P\left(w_{3}\left(w_{1} w_{2}\right))\right.$
+ 2 gram: $P\left(w_{1}|<s>\right) P\left(w_{2} \mid w_{1}\right) P\left(w_{3} \mid w_{2}\right) \cdots$,
  +  $|V| \times|V|$ parameters (each word to each word)
  + Problem: very flat distribution, eg. same $P(w|the)$ for many $w$

+ Solution: parameter estimation: $P(\text { dog } \mid \text { the })=\frac{\text { count }(\text { the dog })}{\text { count }(\text { the })}$
  + Generative machine translation
  + grammatical error correction
  + way to build "word2vec++"
+ Problem with higher n-gram (n>5): 0 probability for many pairs
  + eg: $P(\text { Austin } \mid \text { want to go to })=0$ is corpus isn't large
+ Solution: Absolute Discounting (Kneser-Ney smoothing):

#### Absolute Discounting (Kneser-Ney smoothing)

$$
P_{A 0}(\text { Austin } \mid \text { want to go to })=\frac{\operatorname{cov} n t(wtgtA)-K}{\text { covut }(w t g t)}+\lambda P_{A D}(A \mid \operatorname{tg} t)\\
K: \text{discount factor, eg. 2}\\
\lambda = \frac{K*\text{num of word types in context}}{\text{num words in context}}\\
P_{A D}(A \mid \operatorname{tg} t): \text{n-1 gram, all the way to } P(A)
$$

For "want to go to":

|                      | Maui | class | campus |
| -------------------- | ---- | ----- | ------ |
| num seen             | 2    | 1     | 1      |
| after discount K=0.2 | 1.8  | 0.8   | 0.8    |
| $\lambda = 0.2*3/4$  |      |       |        |

### Neural Language Models

$$
P(\bar{w})=P\left(w_{1}\right) P\left(w_{2} \mid w_{1}\right) \cdots \underbrace{P\left(w_{n} \mid w_{1} \ldots w_{n-1}\right)}_{\text {model with a neural net }}\\
\text{option 1:}\\
P\left(w_{i} \mid w_{i-1}\right)=\frac{e^{v_{i}\cdot c_{i-1}}}{\sum_{w^{\prime} \in V} e^{v_{w} \cdot c_{i-1}}}\\
\text{option 2:}\\
P\left(w_{i} \mid w_{1} \ldots w_{i-1}\right)=\operatorname{sottmax}\left(U_{w_{i}} \cdot f\left(w_{1}, \ldots, v_{i}\right)\right)
$$

+ $f$: eg. DAN, FFNN
+ Problem with above model: 
  + DAN: averaged, no information on order. 
  + FFNN: if concat all word vectors together, each position in the feature vector has fixed semantics, but they may be the same word

#### RNN

+ Basic idea:
  + x: input
  + h: hidden state
  + c: cell state
  + y: output
    <img src="/home/arkyyang/files/notes/notes/attachments/image-20210223140123272.png" alt="image-20210223140123272" style="zoom:67%;" />

+ Transducer: make some prediction for each element in a sequence

<img src="/home/arkyyang/files/notes/notes/attachments/image-20210223140159209.png" alt="image-20210223140159209" style="zoom:67%;" />

+ Acceptor/encoder: encode a sequence into a fixed-sized vector and use that for some purpose

<img src="/home/arkyyang/files/notes/notes/attachments/image-20210223140238473.png" alt="image-20210223140238473" style="zoom:67%;" />

##### Elman Networks

$$
\mathbf{h}_{t}=\tanh \left(W \mathbf{x}_{t}+V \mathbf{h}_{t-1}+\mathbf{b}_{h}\right)\\
\mathbf{y}_{t}=\tanh \left(U \mathbf{h}_{\mathrm{t}}+\mathbf{b}_{y}\right)
$$

+ Training: “Backpropagation through time": build the network as one big computation graph, some parameters are shared.  RNN potentially needs to learn how to "remember" information for a long time! 

+ Problem: vanishing/ exploding gradient: $h_t = ...(Vh_{t-1}...)$, same operation on $h$ repeatedly
  <img src="/home/arkyyang/files/notes/notes/attachments/image-20210223140606393.png" alt="image-20210223140606393" style="zoom:67%;" />

##### Gated Connections

+ $\mathbf{f}$ : forget gate
  + sigmoid forces value to be in (0,1)
  + if $\mathbf{f} \approx 1$: simple sum over all $\mathbf{h_t}$, gradient doesn't vanish, no multiplication with $V$

$$
\mathbf{h}_{t}=\mathbf{h}_{t-1} \odot \mathbf{f}+\operatorname{func}\left(\mathbf{x}_{t}\right)\\
\mathbf{f}=\sigma\left(W^{x f} \mathbf{x}_{t}+W^{h f} \mathbf{h}_{t-1}\right)\\
$$

#### LSTM

+ Long short -term memory
  + Hidden state: short-term
  + cell state: combine long term with short term
  + forget gate: depends on hidden state
  + Basic communication flow: $\mathbf{x}->\mathbf{c}->\mathbf{h}->$ output, each step of this process is gated in addition to gates from previous timesteps
  + possible operations:
    + ignore recurrent state entirely: feedforward layer dominates
    + throw out a previous value of $\mathbf{c}$
    + sum up all $\mathbf{x}$: bag-of-words representation
    + ignore a particular  $\mathbf{x}$: discard stopwords
  + Advantage: 
    + gradient diminishes more controllably (initialize forget gage to 1 to remember everything to start)

$$
\begin{array}{l}
\text{states:}\\
\mathbf{c}_{\mathbf{j}}=\mathbf{c}_{\mathbf{j}-\mathbf{1}} \odot \mathbf{f}+\mathbf{g} \odot \mathbf{i} \\
\mathbf{h}_{\mathbf{j}}=\tanh \left(\mathbf{c}_{\mathbf{j}}\right) \odot \mathbf{o} \\
\text{main computation:}\\
\mathbf{g}=\tanh \left(\mathbf{x}_{\mathbf{j}} \mathbf{W}^{\mathbf{x g}}+\mathbf{h}_{\mathbf{j}-\mathbf{1}} \mathbf{W}^{\mathbf{h g}}\right) \\
\text {gates:}\\
\mathbf{f}=\sigma\left(\mathbf{x}_{\mathbf{j}} \mathbf{W}^{\mathbf{x f}}+\mathbf{h}_{\mathbf{j}-\mathbf{1}} \mathbf{W}^{\mathbf{h f}}\right) \\
\mathbf{i}=\sigma\left(\mathbf{x}_{\mathbf{j}} \mathbf{W}^{\mathbf{x} \mathbf{i}}+\mathbf{h}_{\mathbf{j}-\mathbf{1}} \mathbf{W}^{\mathbf{h i}}\right) \\

\mathbf{o}=\sigma\left(\mathbf{x}_{\mathbf{j}} \mathbf{W}^{\mathbf{x} \mathbf{0}}+\mathbf{h}_{\mathbf{j}-\mathbf{1}} \mathbf{W}^{\mathbf{h o}}\right)
\end{array}
$$

<img src="/home/arkyyang/files/notes/notes/attachments/image-20210224111306952.png" alt="image-20210224111306952" style="zoom:67%;" />



#### Other Units

+ Gated recurrent unit (GRU): simpler architecture without $c$

$$
\begin{array}{l}
z_{t}=\sigma_{g}\left(W_{z} x_{t}+U_{z} h_{t-1}+b_{z}\right) \quad \text { "update" } \\
r_{t}=\sigma_{g}\left(W_{r} x_{t}+U_{r} h_{t-1}+b_{r}\right) \quad \text { "reset" } \\
h_{t}=\left(1-z_{t}\right) \circ h_{t-1}+z_{t} \circ \sigma_{h}\left(W_{h} x_{t}+U_{h}\left(r_{t} \circ h_{t-1}\right)+b_{h}\right)
\end{array}
$$
+ Other variants of LSTMs: multiplicative LSTMs, rotational unit of memory (RUM), ...
+ Quantized (binary/ternary weights) and other compressed forms



### RNN - interpretation

<img src="/home/arkyyang/files/notes/notes/attachments/image-20210224112051461.png" alt="image-20210224112051461" style="zoom:67%;" />

+ Transformation of a sequence of vectors into a sequence of context-dependent vectors:
  + output at each time stamp: encoding of each word
  + final output: encoding of the sentence

#### Multilayer Bidirectional RNN

![image-20210224112221132](/home/arkyyang/files/notes/notes/attachments/image-20210224112221132.png)

![image-20210224112243111](/home/arkyyang/files/notes/notes/attachments/image-20210224112243111.png)

### Training RNNs

<img src="/home/arkyyang/files/notes/notes/attachments/image-20210224112412373.png" alt="image-20210224112412373" style="zoom:50%;" /> <img src="/home/arkyyang/files/notes/notes/attachments/image-20210224112429376.png" alt="image-20210224112429376" style="zoom:50%;" />

+ Sentence-wise:
  + Loss = negative log likelihood of probability of gold label
  + Backpropagate through entire network, RNN parameters get a gradient update from each timestep
  - Example: sentiment analysis
+ Word-wise:
  - Loss = negative log likelihood of probability of gold predictions, summed over the tags
  - Loss terms filter back through network
  - Example: language modeling, POS tagging

#### RNN Language Modeling

<img src="/home/arkyyang/files/notes/notes/attachments/image-20210224113046767.png" alt="image-20210224113046767" style="zoom:50%;" /> <img src="/home/arkyyang/files/notes/notes/attachments/image-20210224113109867.png" alt="image-20210224113109867" style="zoom:50%;" /><img src="/home/arkyyang/files/notes/notes/attachments/image-20210224113206740.png" alt="image-20210224113206740" style="zoom:50%;" /> <img src="/home/arkyyang/files/notes/notes/attachments/image-20210224113321979.png" alt="image-20210224113321979" style="zoom:67%;" />
$$
P(w \mid \text { context })=\frac{\exp \left(\mathbf{w} \cdot \mathbf{h}_{\mathbf{i}}\right)}{\sum_{w^{\prime}} \exp \left(\mathbf{w}^{\prime} \cdot \mathbf{h}_{\mathbf{i}}\right)} = \operatorname{softmax}\left(W \mathbf{h}_{i}\right)\\
\text { loss }=-\log P\left(w^{*} \mid \text { context }\right)\\
$$

+ $W$: (vocab size) x (hidden size)
+ Linear layer in PyTorch
+ Input: sequence of words, output: shifted by one
+ Multiple sequences and multiple timesteps per sequence:
  + train on predictions across several timesteps simultaneously (not batching)
  + Batch over different chunks of words (sequences)

+ Total loss = sum of negative log likelihoods at each position
  In PyTorch: simply add the losses together and call .backward()
+ Implementation:
  + torch.nn.Embedding: map sequence of word indicies to vectors [126, 285]->[[0.1,-0.07, 1.2], [-2.3, 0.2, 1.4]], from [seq len, dim] or [batch. seq len] -> [batch, seq len, dim]
  + torch.nn.LSTM / torch.nn.GRU: expect input in [seq len, batch, word dim] format, eg. [4,2,dim] in following
  + batches processed in parallel, not timestamps. Because time stamp depends on previous time stamps

<img src="/home/arkyyang/files/notes/notes/attachments/image-20210224114022890.png" alt="image-20210224114022890" style="zoom:67%;" />

##### Perplexity

+ Accuracy is not a good metric: very unlikely for prediction to match gold label in predicting next word (huge space of word candidates)

+ Use log likelihood of held-out data: $\frac{1}{n} \sum_{i=1}^{n} \log P\left(w_{i} \mid w_{1}, \ldots, w_{i-1}\right)$

+ Perplexity: log perplexity = geometric mean of denominators of probs of each predictions, e.g. probs: 1/4, 1/3, 1/4, 1/4, log perplexity = (3+4+3+4)/4
  $$
  \text { perplexity }(\text { test set } \boldsymbol{w})=\exp \left\{-\frac{\mathcal{L}(\boldsymbol{w})}{\text { count of tokens }}\right\}
  $$
  





### Visualizing LSTMS

Examples

+ "\n" activation

![image-20210224150953139](/home/arkyyang/files/notes/notes/attachments/image-20210224150953139.png)

+ Binary switch for in/ out of quote

![image-20210224151019736](/home/arkyyang/files/notes/notes/attachments/image-20210224151019736.png)

### Applications

#### Context-dependent embeddings

+ Use LSTM to generate embeddings for words, then they will have context information

+ Use LSTM to generate embeddings for words, then they will have context information

#### ELMo

![image-20210224151548199](/home/arkyyang/files/notes/notes/attachments/image-20210224151548199.png)

<img src="/home/arkyyang/files/notes/notes/attachments/image-20210224151933049.png" alt="image-20210224151933049" style="zoom:50%;" />![image-20210224151951298](/home/arkyyang/files/notes/notes/attachments/image-20210224151951298.png)

+ Frozen embeddings: only update weights of network but not ELMo's parameters
+ Find tuning: update both

