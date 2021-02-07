## NLP Intro

### General Pipeline

+ Input: Text
+ Text Analysis-> annotations
  + Syntactic parses
  + co-reference resolution
  + entity disambiguation
  + discourse analysis
+ Applications

### Text representation

+ labels: positive/ negative; objective/subjective
+ sequences/ tags: person/ work_or_art..
+ trees: extract syntactic features, tree-structured neural networks, tree transuders
+ functions
+ End to end models

### ML Basics

#### Gradient Descent:

+  $\mathbf{a}_{n+1}=\mathbf{a}_{n}-\gamma \nabla F\left(\mathbf{a}_{n}\right)$
+ Objective: minimize loss ($F$)
+ $a_n$: $x_n, y_n, \bar{w}_n$
+ gradient over weight vector

#### Perceptron

+ Algorithm:
  $$
  \text{ for t up to epochs:}\\
  \quad \text{ for i up to D: }\\
  
  \quad y_{pred} = \bar{w}^Tf(x^i)>0 \\
  \quad \text{ if } y_{pred}==y^i: \bar{w} = \bar{w} \\
  \quad \text{ if } y^i==1: \bar{w} = \bar{w}+\alpha f(x^i) \\
  \quad\text{ if } y^i==-1: \bar{w} = \bar{w}-\alpha f(x^i) \\
  $$

+ Example

  + |            | y    | f(x)                  | w         | w_prev*f(x) | pred |
    | ---------- | ---- | --------------------- | --------- | ----------- | ---- |
    | [epoch 1]  |      | [movie, good,bad,not] | 0,0,0,0   |             |      |
    | movie good | 1    | 1,1,0,0               | 1,1,0,0   | 0           | -1   |
    | movie bad  | -1   | 1,0,1,0               | 0,1,-1,0  | 1,0,0,0     | 1    |
    | not good   | -1   | 0,1,0,1               | 0,0,-1,-1 | 0,1,0,0     | 1    |
    | [epoch 2]  |      |                       |           |             |      |
    | movie good | 1    | 1,1,0,0               | 1,1,-1,-1 | 0,0,0,0     | -1   |
    | movie bad  | -1   | 1,0,1,0               | Correct   | 1,0,-1,0    | -1   |

  + |          | y    | [good, bad, not] | w      | w_prev*f(x) | pred |
    | -------- | ---- | ---------------- | ------ | ----------- | ---- |
    | good     | 1    | 1,0,0            | 1,0,0  | 0,0,0       | -1   |
    | bad      | -1   | 0,1,0            | 1,0,0  | 0,0,0       | -1   |
    | not good | -1   | 1,0,1            | 0,0,-1 | 1,0,0       | 1    |
    | not bad  | 1    | 0,1,1            | 0,1,0  | 0,0,-1      | -1   |

    Perceptron doesn't word because it is not separable

    <img src="/home/arkyyang/files/notes/notes/attachments/image-20210119151153307.png" alt="image-20210119151153307" style="zoom:50%;" />

  + solution for non-separable problem: change feature space (ex. 2-gram)

  + |           | y    | f(x)           | w      | w_prev*f(x) | pred |
    | --------- | ---- | -------------- | ------ | ----------- | ---- |
    | [epoch 1] |      | [good,bad,not] | 0,0,0  |             |      |
    | good      | 1    | 1,0,0          | 1,0,0  | 0,0,0       | -1   |
    | not good  | -1   | 1,0,1          | 0,0,-1 | 1,0,0       | 1    |
    | bad       | -1   | 0,1,0          | 0,0,-1 | 0,0,0       | -1   |
    | [epoch 2] |      |                |        |             |      |
    | good      | 1    | 1,0,0          | 1,0,-1 | 0,0,0       | -1   |
    | not good  | -1   | 1,0,1          | 1,0,-1 | 1,0,-1      | -1   |
    | bad       | -1   | 0,1,0          | 1,0,-1 | 0,0,0       | -1   |
    | [epoch 3] |      |                |        |             |      |
    | good      | 1    | 1,0,0          | 1,0,-1 | 1,0,0       | 1    |

+ Perceptron as gradient descent

  + Loss: 
    $$
    \operatorname{loss}\left(\bar{x}^{(i)}, y^{(i)}, \bar{w}\right)=\left\{\begin{array}{c}
    0 \quad \text { if } \bar{w}^{\top} f\left(\bar{x}^{(i)}\right) y^{(i)} \geq 0 \\
    -\bar{w}^{\top} f\left(\bar{x}^{(i)}\right) y^{(i)} \text { else }
    \end{array}\right.
    $$

  + take y=1 for example: 

  + $$
    \nabla_w\operatorname{loss}\left(\bar{x}^{(i)}, y^{(i)}, \bar{w}\right)=\left\{\begin{array}{c}0 \quad \text { if } \bar{w}^{\top} f\left(\bar{x}^{(i)}\right) \geq 0 \\- f\left(\bar{x}^{(i)}\right)\text { else }\end{array}\right.
    $$

  + Negative direction of gradient  ($+f(x^i)$) is the perceptron update direction
    <img src="/home/arkyyang/files/notes/notes/attachments/image-20210119151557713.png" alt="image-20210119151557713" style="zoom:50%;" />

#### Logistic Regression + Log Likelihood Maximization

+ Logistic Regression:

  + Discriminative probabilistic model (conditional): $P(y|x)$
  + $P(y=+1 \mid \bar{x})=\frac{e^{\bar{w}^{\top} f(\bar{x})}}{1+e^{\bar{w}^{\top} f(\bar{x})}}$, $P(y=-1 \mid \bar{x})=\frac{1}{1+e^{w^{\top}+(x)}}$, return +1 if $P(y=+1|\bar{x}>0.5)$
  + ![Logistic Regression: Calculating a Probability](https://developers.google.com/machine-learning/crash-course/images/LogisticRegressionOutput.svg)

+ Log Likelihood Maximization: LLM

  + Maximize $\prod_{i=1}^{D} P\left(y^{(i)} \mid \bar{x}^{(i)}\right)$

  + Take log and negative: $\min _{w} \sum_{i=1}^{0} \underbrace{-\log P\left(y^{(i)} \mid \bar{x}^{(i)}\right)}_{\operatorname{loss}\left(\bar{x}^{(i)}, y^{(i)}, \bar{w}\right)}$

  + GD: $\frac{\partial}{\partial \bar{w}} \operatorname{loss}\left(\bar{x}^{(i)}, y^{(i)}, \bar{w}\right)$, for case when $y^i==1$:, take $z=e^{w^Tf(x)}$

  + $$
    \frac{\partial}{\partial \bar{w}} (-\log (z/(1+z)) = \frac{\partial}{\partial \bar{w}} (-wf(x)+log(1+z))\\
    =-f(x)+\dfrac{z*f(x)}{1+z} = f(x)(\dfrac{z}{1+z}-1)\\
    =f(x)(P^+-1)\\
    \text{update:}\\
    w = w+\alpha f(x)(1-P^+)\\
    w = w-\alpha f(x)(1-P^-)\\
    P^+ \approx 1: \text{no update}\\
    P^+ \approx 0: \text{perceptron update}\\
    $$

  + <img src="/home/arkyyang/files/notes/notes/attachments/image-20210119155907246.png" alt="image-20210119155907246" style="zoom:67%;" />



### ML Debug

- Are your inference/prediction computations correct?
  - Check features by hand
  - Check weights computation
  - If you have handset weights (for classifiers), probabilities (for HMMs), etc., is your code doing the right thing?
  - Can you double-check somehow?
- Is the model training?
  - Does your loss decrease? Log objective
  - Train on $1-10$ examples: if you test on these examples, do you get $100 \%$ accuracy?
  - Are you fitting the data fast enough?
- Is the model generalizing?
- If your train performance is okay and dev performance is low: are you underfitting the data? Add more features or increase neural network size
- Print the dimensions of all tensors in your computation graph, particularly when batching. Make sure you understand what all these tensors mean and what dimensions they should be!
- Try making hyperparameters prime numbers wherever possible you can understand where dimensions come from

## Linear Binary Classification

+ Components
  + data: $\bar{x} \in \mathbb{R}^{d}$, 
  + feature vector: $\tilde{f}(\bar{x}) [3,-1,2,(1)]$ last dimension added to prevent using of bias term
  + training set: $\{\tilde{f(\bar{x^i})}, y^i \}_{i=1}^D$
  + label: $y\in \{-1,+1\}$
  + classifier: weight vector $\bar{w}$
  + criteria: $\bar{w}\tilde{f}(\bar{x})\ge 0$ for positive samples

### Pipeline

+ Preprocessing

  + Tokenization: remove punctuations/ spaces, separate negations (wasn't-> was-sn't)
  + Stop-word removal: the, of ,a..
  + Casing
  + handling unknown words: khkjghjg->UNK
  + Indexing: map each word/ n-gram into $\mathbb{N}$ 

+ Feature extraction

  + Bag of words, vocabulary `word` of size $N$, then feature vector is array of size $N$ where `vector[i]=counts or presence of word[i] in sentence`

  + Bag-of-ngrams: n-size phrases

  + tf-idf: 
    $$
    \begin{array}{c}
    w_{i, j}=t f_{i, j} \times \log \left(\frac{N}{d f_{i}}\right) \\
    t f_{i j}=\text { number of occurrences of } i \text { in } j \\
    d f_{i}=\text { number of documents containing } \\
    N=\text { total number of documents }
    \end{array}
    $$

+ 



## Faireness in classification

+ T. Anne Cleary $(1966-1968):$ a test is biased if prediction on a subgroup makes consistent nonzero prediction errors compared to the aggregate
  + ![image-20210126130221058](/home/arkyyang/files/notes/notes/attachments/image-20210126130221058.png)
+ Thorndike (1971), Petersen and Novik (1976): fairness in classification: ratio of predicted positives to ground truth positives must be approximately the same for each group
  - Group $1: 50 \%$ positive movie reviews. Group $2: 60 \%$ positive movie reviews
  - A classifier classifying $50 \%$ positive in both groups is unfair, regardless of accuracy
+ Allows for different criteria across groups: imposing different classification thresholds actually can give a fairer result





## Applications

### Textual Entailment

+ A soccer game with multiple males playing.
  ENTAILS
  Some men are playing a sport.
+ A black race car starts up in front of a crowd of people.
  CONTRADICTS
  A man is driving down a lonely road
+ A smiling costumed woman is holding an umbrella.
  NEUTRAL
  A happy woman in a fairy costume holds an umbrella.

### Entity Linking

![image-20210126135940209](/home/arkyyang/files/notes/notes/attachments/image-20210126135940209.png)



### Authorship Attribution

![image-20210126140017485](/home/arkyyang/files/notes/notes/attachments/image-20210126140017485.png)

![image-20210126140030284](/home/arkyyang/files/notes/notes/attachments/image-20210126140030284.png)