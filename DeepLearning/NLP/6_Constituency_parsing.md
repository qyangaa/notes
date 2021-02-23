## Constituency Parsing

### Intro

+ Syntax: word order and how words form sentences
  + help with multiple interpretations of words
  + recognize verb-argument structures
  + high level abstraction
+ Constituents:  (S)entence, (N)oun (P)hrases, (V)erb (P)hrases, (P)repositional (P)hrases, and
  more
+ Tree: bottom layer is POS tags
  <img src="/home/arkyyang/files/notes/notes/attachments/image-20210222140526333.png" alt="image-20210222140526333" style="zoom:70%;" /> <img src="/home/arkyyang/files/notes/notes/attachments/image-20210222140612506.png" alt="image-20210222140612506" style="zoom:50%;" />

+ Challenges: different ways of parsing
  + modifier scope: (plastic cup) holder/ (plastic cup holder)
  + complement scope: The students complained to the professor that they didn't understand 
  + coordination scope: The man picked up his hammer and saw

![image-20210222140648868](/home/arkyyang/files/notes/notes/attachments/image-20210222140648868.png)

+ Constituency tests:
  + Substitution by proform (pronoun, do so)
  + Clefting (It was with a spoon that...)
  + Answer ellipsis (What did they eat? the cake) (How? with a spoon)



### Probabilistic Context-Free Grammars (PCFG)

{N, T, S, R}: non-terminals, terminals, start, rules

+ Non-terminals: {S, NP, VP, PP, DT, NN, VBD, IN,NNS}: POS, preterminals

+ terminals: {cats, and, in}

+ rules: (binary, unary.., with probabilities) NP → NP CC NP [0.3]

  + prob sum to one per parent 
    $$
    \begin{array}{rl}
    1 & S \rightarrow N P V P \\
    1 / 4 & V P \rightarrow V B D N P \\
    1 / 2 & N P \rightarrow D T N N \\
    1 / 2 & N P \rightarrow D T \text { NNS }
    \end{array}\\
    P(\text { tree } T)=\prod_{\text {rules }} P(r \mid \text { parent }(r))
    $$

#### Steps to parsing

1. Binarization:
   ![image-20210222141827638](/home/arkyyang/files/notes/notes/attachments/image-20210222141827638.png)
2. Estimation of rule probs (count + normalizing) (maximum likelihood est)
3. Inference: $\arg \max _{T} P(T \mid \bar{x})$

#### Example

NP → NP CC NP [0.3]

NP → NP PP [0.3]

NP → NNS [0.4]

PP → P NP [1.0]

NNS → cats [1.0]

CC → and [1.0]

P → in [1.0]

" Cats and cats": NP CC NP







### CKY Algorithm

![image-20210222150648221](/home/arkyyang/files/notes/notes/attachments/image-20210222150648221.png)
$$
t[i,j,x]\\
t[i, i+1, x] = \log P(w_i|x)\\
\text{over all splitting index:}\\
t[i,j,x] = \max_{i<k<j} \max_{r: x\rightarrow x_1, x_2} \log P(x\rightarrow x_1, x_2 ) + t[i,k,x] + t[k,j,x]
$$

| DT   | ->   | the    | 1.0  |
| ---- | ---- | ------ | ---- |
| NN   | ->   | child  | 1.0  |
| NNS  | ->   | raises | 1.0  |
| VBZ  | ->   | raises | 1.0  |
| PRP  | ->   | it     | 1.0  |

| S    | ->   | NP   | VP   | 1.0  |
| ---- | ---- | ---- | ---- | ---- |
| NP   | ->   | DT   | NN   | .5   |
| NP   | ->   | NN   | NNS  | .5   |
| VP   | ->   | VBZ  | PRP  | 1.0  |

Runtime: $O(n^3G)$ (G is grammar space)



### Problem:

Probability rules are not fixed in different context:

![image-20210222151833720](/home/arkyyang/files/notes/notes/attachments/image-20210222151833720.png)

+ Solution 1: merge higher parent into current parent:
  ![image-20210222151919992](/home/arkyyang/files/notes/notes/attachments/image-20210222151919992.png)

+ Solution 2: incorporate neighbor into current parent:

![image-20210222152011441](/home/arkyyang/files/notes/notes/attachments/image-20210222152011441.png)

+ Horizontal Markovization: only keep a certain distance of depencency along the neighbor - chain
  ![image-20210222152130553](/home/arkyyang/files/notes/notes/attachments/image-20210222152130553.png)

+ Combined solution: 

![image-20210222152218605](/home/arkyyang/files/notes/notes/attachments/image-20210222152218605.png)

### Lexicalization (dependency)

+ Pick the "head word" of children pair and use that to annotate each grammar symbol
  <img src="/home/arkyyang/files/notes/notes/attachments/image-20210222152443069.png" alt="image-20210222152443069" style="zoom:67%;" />

+ dependent-to-head word relationship can be represented as arcs, each word has exactly one parent except for ROOT
  <img src="/home/arkyyang/files/notes/notes/attachments/image-20210222154758295.png" alt="image-20210222154758295" style="zoom:67%;" />

<img src="/home/arkyyang/files/notes/notes/attachments/image-20210222154922985.png" alt="image-20210222154922985" style="zoom:67%;" />![image-20210222155006605](/home/arkyyang/files/notes/notes/attachments/image-20210222155006605.png)

<img src="/home/arkyyang/files/notes/notes/attachments/image-20210222154922985.png" alt="image-20210222154922985" style="zoom:67%;" />![image-20210222155006605](/home/arkyyang/files/notes/notes/attachments/image-20210222155006605.png)



+ Universal dependencies: same dependency annotations across different languages (not for language with free word order)
+ Projectivity: structure with no crossing arcs
  + Example of  non-projective sentence: ![image-20210222155630837](/home/arkyyang/files/notes/notes/attachments/image-20210222155630837.png)

#### Dependency vs. Constituency:

![image-20210222155110938](/home/arkyyang/files/notes/notes/attachments/image-20210222155110938.png)

![image-20210222155259086](/home/arkyyang/files/notes/notes/attachments/image-20210222155259086.png)

#### Algorithm for dependency:

+ Stack + Buffer

<img src="/home/arkyyang/files/notes/notes/attachments/image-20210222163619255.png" alt="image-20210222163619255" style="zoom:67%;" />![image-20210222163632986](/home/arkyyang/files/notes/notes/attachments/image-20210222163632986.png)

<img src="/home/arkyyang/files/notes/notes/attachments/image-20210222163619255.png" alt="image-20210222163619255" style="zoom:67%;" />![image-20210222163632986](/home/arkyyang/files/notes/notes/attachments/image-20210222163632986.png)![image-20210222163646575](/home/arkyyang/files/notes/notes/attachments/image-20210222163646575.png)

<img src="/home/arkyyang/files/notes/notes/attachments/image-20210222164404010.png" alt="image-20210222164404010" style="zoom:67%;" />

+ We need 2n (n shifts + n arcs) transitions
+ Always do left arc as early as we can (not with ROOT). When buffer is empty, start to do right arc

+ Example:

<img src="/home/arkyyang/files/notes/notes/attachments/image-20210222164649606.png" alt="image-20210222164649606" style="zoom:50%;" /> <img src="/home/arkyyang/files/notes/notes/attachments/image-20210222164706519.png" alt="image-20210222164706519" style="zoom:50%;" />

+ Find right decisions: multi-way classification: $\operatorname{argmax}_{a \in\{\mathrm{S}, \mathrm{LA}, \mathrm{RA}\}} w^{\top} f(\text { stack, buffer, } a)$
+ Challenge: feature design
  + Things to look at: top words/POS of buffer, top words/POS of stack, leftmost and rightmost children of top items on the stack

Example:

+ [] [The lazy cat slept] shift
+ [The] [lazy cat slept]  shift
+ [The lazy ] [cat slept] shift
+ [The lazy cat ] [slept] left-arc, left-arc
+ [cat] [slept] shift
+ [cat slept] left arc
+ [slept] right-arc
+ []



#### Training

+ Greedy model: Turn a tree into an oracle. 
  + Issue: only start at correct solution at each node, no idea how to recover if under mistake (Non-gold states unobserved during training: consider making bad decisions but don't condition on bad decisions)