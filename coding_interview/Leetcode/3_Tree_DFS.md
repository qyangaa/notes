### 98 Validate Binary Search Tree

Given the `root` of a binary tree, *determine if it is a valid binary search tree (BST)*.

A **valid BST** is defined as follows:

- The left subtree of a node contains only nodes with keys **less than** the node's key.
- The right subtree of a node contains only nodes with keys **greater than** the node's key.
- Both the left and right subtrees must also be binary search trees.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/12/01/tree1.jpg)

```
Input: root = [2,1,3]
Output: true
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/12/01/tree2.jpg)

```
Input: root = [5,1,4,null,null,3,6]
Output: false
Explanation: The root node's value is 5 but its right child's value is 4.
```

+ Recursion:
  + `isValidBST(left) && isValidBST(right) && max(left)<cur<min(right)`

```python
class Solution:
    def isValidBST(self, root: TreeNode) -> bool:
        return self.inOrder(root)[2]
    def inOrder(self, node: TreeNode):
        curMax = curMin = node.val
        if not node.left and not node.right:
            return (curMax, curMin, 1)
        if node.left:
            leftMax, leftMin, leftValid = self.inOrder(node.left)
            if leftMax>=curMin or leftValid==0:
                return (0,0,0)
            else:
                curMin=leftMin
        if node.right:
            rightMax, rightMin, rightValid = self.inOrder(node.right)
            if rightMin<=curMax or rightValid==0:
                return (0,0,0)
            else:
                curMax=rightMax
        return (curMax, curMin, 1)
```

+ Iterative Traversal:

  ```python
  if not root: return True
  stack=[(root, -math.inf, math.inf)]
  while stack:
      root, lower, upper = stack.pop()
      if not root:
          continue
      val = root.val
      if val<=lower or val>=upper:
          return False
      stack.append((root.right, val, upper))
      stack.append((root.left, lower, val))
  return True
  ```

+ In order traversal

  ```python
  self.prev=-math.inf
  return inorder(root)
  
  def inorder(root):
      if not root:
          return True
      if not inorder(root.left):
          return False
      if root.val <=self.prev:
          return False
      self.prev=root.val
      return inorder(root.right)
  ```

+ In order iterative

  ```python
  stack, prev=[], -math.inf
  while stack or root:
      while root: # put all in stack
          stack.append(root)
          root=root.left
      root = stack.pop()
      if root.val<=prev:
          return False
      prev = root.val
      root = root.right
  ```

  

### 100 Same Tree

Given the roots of two binary trees `p` and `q`, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical, and the nodes have the same value.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/12/20/ex1.jpg)

```
Input: p = [1,2,3], q = [1,2,3]
Output: true
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/12/20/ex2.jpg)

```
Input: p = [1,2], q = [1,null,2]
Output: false
```

**Example 3:**

![img](https://assets.leetcode.com/uploads/2020/12/20/ex3.jpg)

```
Input: p = [1,2,1], q = [1,1,2]
Output: false
```

+ Recursive traversal

  ```python
      def isSameTree(self, p: TreeNode, q: TreeNode) -> bool:
          if not p: return not q
          if not q: return not p
          if not p.val==q.val: return False
          return self.isSameTree(p.left, q.left) and self.isSameTree(p.right, q.right)
  ```

+ Iteration

  ```python
  deq = deque([(p,q)])
  while deq:
      p,q=deq.popleft()
      if not check(p,q):
          return False
      if p:
          deq.append((p.left, q.left))
          deq.append((p.right, q.right))
      return True
  
  def check(p,q):
      if not p and not q: return True
      if not p or not q: return False
      if p.val!=q.val: return False
      return True
  ```

  

### 104 Maximum Depth of Binary Tree

Given the `root` of a binary tree, return *its maximum depth*.

A binary tree's **maximum depth** is the number of nodes along the longest path from the root node down to the farthest leaf node.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/11/26/tmp-tree.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: 3
```

**Example 2:**

```
Input: root = [1,null,2]
Output: 2
```

**Example 3:**

```
Input: root = []
Output: 0
```

**Example 4:**

```
Input: root = [0]
Output: 1
```



+ BFS gives depth of each level
+ Try DFS this time:

```python
from collections import deque
class Solution:
    def maxDepth(self, root: TreeNode) -> int:
        if not root: return 0
        queue = deque([(root,0)])
        maxDepth = 0
        while queue:
            node, depth = queue.popleft()
            if not node:
                maxDepth=max(depth, maxDepth)
                continue
            queue.append((node.left, depth+1))
            queue.append((node.right, depth+1))
        return maxDepth
```



### 105 Construct Binary Tree from Preorder and Inorder Traversal



Given preorder and inorder traversal of a tree, construct the binary tree.

**Note:**
You may assume that duplicates do not exist in the tree.

For example, given

```
preorder = [3,9,20,15,7]
inorder = [9,3,15,20,7]
```

Return the following binary tree:

```
    3
   / \
  9  20
    /  \
   15   7
```

+ Traverse over preorder:
  + Then put subsequent left and right children based on their order in the in-order list
  + go over the inorder list and create a dictionary for fast read of location
  + recursion: 
    `node(3).left = tree(9)`
    `node(3).right = tree(15,20,7)`

+ Naive recursion

  ```python
  class Solution:
      def buildTree(self, preorder: List[int], inorder: List[int]) -> TreeNode:
          if len(preorder)==0:
              return None
          root=TreeNode(preorder[0])
          middle=inorder.index(preorder[0])
          root.left=self.buildTree(preorder[1:middle+1],inorder[:middle])
          root.right=self.buildTree(preorder[middle+1:],inorder[middle+1:])
          return root
  ```


+ Pass range, save memory but slower

  ```python
  class Solution:
      def buildTree(self, preorder: List[int], inorder: List[int]) -> TreeNode:
          def build(prel,prer, inl, inr):
              if prel>prer or inl>inr: return None
              val = preorder[prel]
              node = TreeNode(val)
              for i in range(inl, inr+1):
                  if inorder[i] == val:
                      idx = i
                      break
              node.left = build(prel+1, prel + (idx-inl) , inl, idx-1)
              node.right = build(prel + (idx-inl)+1, prer, idx+1, inr)
              return node
          
          n = len(preorder)
          return build(0,n-1,0,n-1)
          
  ```

  ```java
  class Solution {
      public TreeNode buildTree(int[] preorder, int[] inorder) {
          int prel = 0;
          int prer = preorder.length -1;
          int inl = 0;
          int inr = inorder.length -1;
          return build(preorder, prel, prer, inorder, inl, inr);
          
      }
      
      TreeNode build(int[] preorder, int prel, int prer, int[] inorder, int inl, int inr){
          if (prel > prer || inl > inr)
              return null;
          int val = preorder[prel];
          TreeNode node = new TreeNode(val);
          int idx = 0;
          for (int i = inl; i <= inr; i++){
              if (inorder[i] == val){
                  idx = i;
                  break;
              }
          }
          
          node.left = build(preorder, prel+1, prel + (idx-inl), inorder, inl, idx-1);
          node.right = build(preorder, prel + (idx-inl)+1, prer , inorder, idx+1, inr);
          return node;
      }
      
  }
  ```

  

### 106 Construct Binary Tree from Inorder and Postorder Traversal

```
inorder = [9,3,15,20,7]
postorder = [9,15,7,20,3]
```

```python
class Solution:
    def buildTree(self, inorder: List[int], postorder: List[int]) -> TreeNode:
        def build(pl,pr,inl, inr):
            if (pl>pr or inl > inr): return None
            val = postorder[pr]
            node = TreeNode(val)
            for i in range(inl, inr+1):
                if val == inorder[i]:
                    idx = i
            node.left = build(pl, pl+ idx-inl-1, inl, idx-1)
            node.right = build(pl+idx-inl,pr-1, idx+1, inr)
            return node
        
        n = len(inorder)
        return build(0, n-1, 0, n-1)
```

```java
class Solution {
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        int pl = 0;
        int pr = inorder.length-1;
        int inl = 0;
        int inr = inorder.length-1;
        return build(postorder, pl, pr, inorder, inl, inr);
    }
    
    TreeNode build(int[] postorder, int pl, int pr, int[] inorder, int inl, int inr){
        if (pl>pr || inl>inr){
            return null;
        }
        int val = postorder[pr];
        TreeNode node = new TreeNode(val);
        int idx = 0;
        for (int i = inl ;i <= inr; i++){
            if (inorder[i] == val){
                idx = i;
                break;
            }
        }
        node.left = build(postorder, pl, pl+idx-inl-1, inorder, inl, idx-1);
        node.right = build(postorder, pl+idx-inl, pr-1, inorder, idx+1, inr);
        return node;
    }
}
```







### 112 Path Sum

Given a binary tree and a sum, determine if the tree has a root-to-leaf path such that adding up all the values along the path equals the given sum.

**Note:** A leaf is a node with no children.

**Example:**

Given the below binary tree and `sum = 22`,

```
      5
     / \
    4   8
   /   / \
  11  13  4
 /  \      \
7    2      1
```

return true, as there exist a root-to-leaf path `5->4->11->2` which sum is 22.

+ A path terminates when no left child and no right child

+ simple recursion is fast enough:

  ```python
  class Solution:
      def hasPathSum(self, root: TreeNode, sum: int) -> bool:
          if not root: return False
          remaining = sum-root.val
          if remaining==0 and not root.left and not root.right: return True
          
          return self.hasPathSum(root.left,remaining) or self.hasPathSum(root.right, remaining)
  ```

  

### 113 Path Sum II

Given a binary tree and a sum, find all root-to-leaf paths where each path's sum equals the given sum.

**Note:** A leaf is a node with no children.

**Example:**

Given the below binary tree and `sum = 22`,

```
      5
     / \
    4   8
   /   / \
  11  13  4
 /  \    / \
7    2  5   1
```

Return:

```
[
   [5,4,11,2],
   [5,8,4,5]
]
```

+ Use mark parent method to trace back the path. And copy the method in finding if pathSum exists

  ```python
  from collections import deque
  class Solution:
      def __init__(self):
          self.paths=[]
      
      def pathSum(self, root: TreeNode, sum: int) -> List[List[int]]:
          if not root: return []
          root.par = None
          self.hasSum(root,sum)
          return self.paths
  
      
      def hasSum(self, node, sum):
          if not node: return
          remaining = sum-node.val
          if remaining==0 and not node.left and not node.right: 
              path = deque([node.val])
              while node.par:
                  path.appendleft(node.par.val)
                  node=node.par
              self.paths.append(path)
              return
          if node.left:
              node.left.par = node
          if node.right:
              node.right.par=node
          
          self.hasSum(node.left,remaining)
          self.hasSum(node.right, remaining)
          return
  ```

  

### 124 Binary Tree Maximum Path Sum

Given the `root` of a binary tree, return *the maximum path sum*.

For this problem, a path is defined as any node sequence from some starting node to any node in the tree along the parent-child connections. The path must contain **at least one node** and does not need to go through the root.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/10/13/exx1.jpg)

```
Input: root = [1,2,3]
Output: 6
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/10/13/exx2.jpg)

```
Input: root = [-10,9,20,null,null,15,7]
Output: 42
```



+ It is more like a connected graph problem. Then we mark the parent first

+ Brute force: $O(2^n)$ whether a node is in the path, and if the path is connected

+ Divide and conqur: many cases to consider, worked out after debugging:

  ```python
  class Solution: 
      def maxPathSum(self, root: TreeNode) -> int:
          self.maxres = root.val 
          self.maxres=max(self.maxSum(root), self.maxres)
          return self.maxres
      def maxSum(self, root: TreeNode):
          if not root: return 0
          if not root.left and not root.right: return root.val
          val = root.val
          left = self.maxSum(root.left)
          right = self.maxSum(root.right)
          print(val, left,right)
          if not root.left:
              # not propagating up
              self.maxres = max(right, self.maxres)
              #propagate up
              return max(right+val, val)
          if not root.right:
              # not propagating up
              self.maxres = max(left, self.maxres)
              #propagate up
              return max(left+val,val)
          # not propagating up
          self.maxres = max(left,right, self.maxres, left+right+val)
          #propagate up
          return max(val, left+val, right+val)
  ```

  

### 208 Implement Trie (prefix tree)

Implement a trie with `insert`, `search`, and `startsWith` methods.

**Example:**

```
Trie trie = new Trie();

trie.insert("apple");
trie.search("apple");   // returns true
trie.search("app");     // returns false
trie.startsWith("app"); // returns true
trie.insert("app");   
trie.search("app");     // returns true
```

**Note:**

- You may assume that all inputs are consist of lowercase letters `a-z`.
- All inputs are guaranteed to be non-empty strings.
- ![Data Structures Tutorials - Tries with an example](https://lh3.googleusercontent.com/proxy/8kJAwWEn0fu817GxrQ9BOc_dXKE0B9wx97YlFYJbFggEadcNULvJr-EGKFqXhRFnXKmPX2kFTbDuysGW5QJI4I6RHyfXc67k_DxtXeGIIg2q7yPblR08sTXihm7ODtM7tQ)

```python
class TrieNode:
    def __init__(self, value):
        self.char=value
        self.children={}

class Trie:
    


    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.head = TrieNode("")
        

    def insert(self, word: str) -> None:
        """
        Inserts a word into the trie.
        """
        if not word: return
        node = self.head
        for c in word:
            if c not in node.children:
                node.children[c] = TrieNode(c)
            node=node.children[c]
        node.children["#"]=TrieNode("#")
            
        
        

    def search(self, word: str) -> bool:
        """
        Returns if the word is in the trie.
        """
        node = self.head
        for c in word:
            if c not in node.children: return False
            node = node.children[c]
        return "#" in node.children
        

    def startsWith(self, prefix: str) -> bool:
        """
        Returns if there is any word in the trie that starts with the given prefix.
        """
        node = self.head
        for c in prefix:
            if c not in node.children: return False
            node = node.children[c]
        return True
```



### 212 Word Search II

Given an `m x n` `board` of characters and a list of strings `words`, return *all words on the board*.

Each word must be constructed from letters of sequentially adjacent cells, where **adjacent cells** are horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/11/07/search1.jpg)

```
Input: board = [["o","a","a","n"],["e","t","a","e"],["i","h","k","r"],["i","f","l","v"]], words = ["oath","pea","eat","rain"]
Output: ["eat","oath"]
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/11/07/search2.jpg)

```
Input: board = [["a","b"],["c","d"]], words = ["abcb"]
Output: []
```

+ Start from an index. Find all words starting with that character. Then task becomes checking if DFS gives the word

+ Simple iteration over the board and words gave ok run time.  $O((m*n)^2*n)$, worst case similar to backtracking solution. But space is much worse

+ ```python
  class Solution:
      def findWords(self, board: List[List[str]], words: List[str]) -> List[str]:
          self.board = board
          self.m = len(board)
          self.n = len(board[0])
          hasWords=set()
          for i in range(self.m):
              for j in range(self.n):
                  for word in words:
                      visited=set()
                      if self.findStartFrom((i,j),word, visited):
                          hasWords.add(word)
          return hasWords
      def findStartFrom(self, idx, word,visited):
          if not word: return True
          c = self.board[idx[0]][idx[1]]
          if c!=word[0]: 
              return False
          elif len(word)==1:
              return True
          contains=False
          newVisited = visited.copy()
          newVisited.add(idx)
          for nextIdx in self.legalSteps(idx,visited):
              if self.findStartFrom(nextIdx, word[1:],newVisited):
                  contains=True
          return contains
      
      def legalSteps(self, idx,visited):
          legalSteps=[]
          ic,jc = idx
          for i,j in [(ic+1,jc),(ic-1,jc),(ic,jc+1),(ic,jc-1)]:
              if i>=self.m or j>=self.n or i<0 or j<0 or (i,j) in visited:
                  continue
              legalSteps.append((i,j))
          return legalSteps
          
  ```




### 226 Invert Binary Tree

Invert a binary tree.

**Example:**

Input:

```
     4
   /   \
  2     7
 / \   / \
1   3 6   9
```

Output:

```
     4
   /   \
  7     2
 / \   / \
9   6 3   1
```

+ Recursion: invert left-right child going down

```python
class Solution:
    def invertTree(self, root: TreeNode) -> TreeNode:
        if not root: return root
        root.left, root.right = root.right, root.left
        self.invertTree(root.right)
        self.invertTree(root.left)
        return root
```

+ Iteration: less memory but double the time

```python
if not root return root
queue=deque([root])
while queue:
    cur = queue.popleft()
    cur.left, cur.right = cur.right, cur.left
    if cur.left: queue.append(cur.left)
    if cur.right: queue.append(cur.right)
return root
```



### 230 Kth Smallest Element in a BST

Given a binary search tree, write a function `kthSmallest` to find the **k**th smallest element in it.

 

**Example 1:**

```
Input: root = [3,1,4,null,2], k = 1
   3
  / \
 1   4
  \
   2
Output: 1
```

**Example 2:**

```
Input: root = [5,3,6,2,4,null,null,1], k = 3
       5
      / \
     3   6
    / \
   2   4
  /
 1
Output: 3
```

+ Simple solution: in-order traversal, stop at k. 

+ ```python
      def kthSmallest(self, root: TreeNode, k: int) -> int:
          def inorder(root):
              return inorder(root.left)+[root.val]+inorder(root.right) if root else []
          return inorder(root)[k-1]
  ```

+ Iterative

  ```python
  class Solution:
      def kthSmallest(self, root: TreeNode, k: int) -> int:
          stack=[]
          while True:
              while root:
                  stack.append(root)
                  root=root.left
              root=stack.pop()
              k-=1
              if not k:
                  return root.val
              root=root.right
  ```

  

### 235 Lowest Common Ancestor of a Binary Search Tree

Given a binary search tree (BST), find the lowest common ancestor (LCA) of two given nodes in the BST.

According to the [definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest_common_ancestor): “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow **a node to be a descendant of itself**).”

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2018/12/14/binarysearchtree_improved.png)

```
Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
Output: 6
Explanation: The LCA of nodes 2 and 8 is 6.
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2018/12/14/binarysearchtree_improved.png)

```
Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
Output: 2
Explanation: The LCA of nodes 2 and 4 is 2, since a node can be a descendant of itself according to the LCA definition.
```

**Example 3:**

```
Input: root = [2,1], p = 2, q = 1
Output: 2
```

+ post order traversal: if inleft() and inright() then return
+ Lowest Common ancestor is when one node in on the left and another node on the right
+ Need to consider the property of BST: if both nodes are larger, then on the right

+ Solution

  + Recursive:

    ```python
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        parent_val, p_val, q_val = root.val, p.val,q.val
        if p_val>parent_val and q_val>parent_val:
            return self.lowestCommonAncestor(root.right, p,q)
        elif p_val<parent_val and q_val<parent_val:
            return self.lowestCommonAncestor(root.left, p,q)
        else:
            return root
    ```

  + Iterative

    ```python
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        p_val, q_val = p.val,q.val
        node=root
        while node:
            parent_val = node.val
            if p_val>parent_val and q_val>parent_val:
                node=node.right
            elif p_val<parent_val and q_val<parent_val:
                node=node.left
            else:
                return node
    ```

    

### 236  Lowest Common Ancestor of a Binary Tree

+ Analysis: 

  + not BST, cannot do value comparison. We need to go into the tree to see if the node is there
  + To prevent repetition, we want to search for two nodes at the same time

+ Method 1: mark parent and traverse back. Can speed up by terminating markParent when find p or q

  ```python
  def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
      if not root: return None
      root.parent=None
      self.markParent(root)
      p_parents = set()
      while p:
          p_parents.add(p)
          p=p.parent
      while q:
          if q in p_parents:
              return q
          q = q.parent
  
  def markParent(self, root):
      if not root: return 
      if root.left:
          root.left.parent = root
          self.markParent(root.left)
      if root.right:
          root.right.parent = root
          self.markParent(root.right)
      return
  ```

  

+ Recursive

  ```python
  def sol(root, p,q):
      ans=root
      def recurse(node):
          if not node: return False
          left=recurse(node.left)
          right=recurse(node.right)
          mid=node==p or node==q
          if mid+left+right>=2 # not only in one branch
          	ans=cur_node
          return mid or left or right
      recurse(root)
      return ans
  ```

  

### 297. Serialize and Deserialize Binary Tree

Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.

**Clarification:** The input/output format is the same as [how LeetCode serializes a binary tree](https://leetcode.com/faq/#binary-tree). You do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/09/15/serdeser.jpg)

```
Input: root = [1,2,3,null,null,4,5]
Output: [1,2,3,null,null,4,5]
```

**Example 2:**

```
Input: root = []
Output: []
```

**Example 3:**

```
Input: root = [1]
Output: [1]
```

**Example 4:**

```
Input: root = [1,2]
Output: [1,2]
```

 

+ Pre-order, for tree, recursion is ok, faster and smaller memory than iteration

  ```python
  from collections import deque
  class Codec:
      def serialize(self, root):
          if not root:
              return '^'
          return str(root.val) + ' ' + self.serialize(root.left) + ' ' + self.serialize(root.right)
          
  
      def deserialize(self, data):
          data = deque(data.split(' '))
          def build(data):
              cur = data.popleft()
              if cur == '^':
                  return None
              node = TreeNode(int(cur))
              node.left = build(data)
              node.right = build(data)
              return node
          return build(data)
  ```

  

```java
public class Codec {

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        
        if (root == null){
            return "^";
        }
        return String.valueOf(root.val) + "," + serialize(root.left) + "," + serialize(root.right);
    }

    TreeNode build(Queue<String> q){
        String cur = q.poll();
        if (cur.equals("^")){
            return null;
        }
        TreeNode node = new TreeNode(Integer.parseInt(cur));
        node.left = build(q);
        node.right = build(q);
        return node;
    }
    
    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        String[] array = data.split(",");
        Queue<String> q = new LinkedList<String>(Arrays.asList(array));
        return build(q);
    }
}
```



### 437 Path Sum III

You are given a binary tree in which each node contains an integer value.

Find the number of paths that sum to a given value.

The path does not need to start or end at the root or a leaf, but it must go downwards (traveling only from parent nodes to child nodes).

The tree has no more than 1,000 nodes and the values are in the range -1,000,000 to 1,000,000.

**Example:**

```
root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8

      10
     /  \
    5   -3
   / \    \
  3   2   11
 / \   \
3  -2   1

Return 3. The paths that sum to 8 are:

1.  5 -> 3
2.  5 -> 2 -> 1
3. -3 -> 11
```

+ Analysis: does not need to start or end at root or leaf. 
+ Recursion:
  + pathSum (root.left, sumVal-root.val)...
  + if sumVal==0, sum+=1

```python
def pathSum(self, root: TreeNode, sum: int) -> int:
    self.numPaths=0
    self.sum = sum
    self.findPath(root, [sum])
    return self.numPaths
def findPath(self, node, sumVals):

    if not node: 
        return
    for val in sumVals: 
        if node.val==val:
            self.numPaths+=1
    sumVals = [val-node.val for val in sumVals]
    sumVals.append(self.sum)
    self.findPath(node.left, sumVals)
    self.findPath(node.right, sumVals)
```

+ Solution: prefix sum

  + *Prefix sum* is a sum of the current value with all previous elements starting from the beginning of the structure.![append](https://leetcode.com/problems/path-sum-iii/Figures/437/prefix_qd.png)

    + ![append](https://leetcode.com/problems/path-sum-iii/Figures/437/2d_prefix.png)
    + ![append](https://leetcode.com/problems/path-sum-iii/Figures/437/tree2.png)

  + Application: Number of Continuous Subarrays that Sum to Target![append](https://leetcode.com/problems/path-sum-iii/Figures/437/situation24.png)
    + ```python
      count = cur_sum=0
      h = defaultdict(int)
      for num in nums:
          cur_sum+=num
          if cur_sum==k:
              count+=1
          count+= h[cur_sum-k]
          h[cur_sum]+=1
      return count
      ```

  + Prefix sum for Path Sum

    ```python
    def pathSum(root, sum):
        count=0
        k=sum
        h=defaultdict()
        def preorder(node, cur_sum):
            if not node: return
            cur_sum+=node.val
            if cur_sum == k: count+=1
            count+= h[cur_sum-k]
            h[cur_sum]+=1
            preorder(node.left, cur_sum)
            preorder(node.right, cur_sum)
            h[cur_sum]-=1 #prevent reuse during parallel subtree processing
        preorder(root, 0)
        return count
    ```

    

### 543. Diameter of Binary Tree

Given a binary tree, you need to compute the length of the diameter of the tree. The diameter of a binary tree is the length of the **longest** path between any two nodes in a tree. This path may or may not pass through the root.

**Example:**
Given a binary tree

```
          1
         / \
        2   3
       / \     
      4   5    
```



Return **3**, which is the length of the path [4,2,1,3] or [5,2,1,3].

**Note:** The length of path between two nodes is represented by the number of edges between them.



+ Longest path passing a root is depth(left)-1+depth(right)-1
+ Recursion to find depth, and keep a record of longest path

```python
def diameterOfBinaryTree(self, root: TreeNode) -> int:
    self.longestPath = 0
    self.findDepth(root,0)
    return self.longestPath
def findDepth(self, root, curDepth):
    if not root:
        return curDepth-1
    left = self.findDepth(root.left, curDepth+1)
    right = self.findDepth(root.right, curDepth+1)
    self.longestPath=max(left+right-2*curDepth, self.longestPath)
    return max(left, right)
```

### 572 Subtree of Another Tree

Given two **non-empty** binary trees **s** and **t**, check whether tree **t** has exactly the same structure and node values with a subtree of **s**. A subtree of **s** is a tree consists of a node in **s** and all of this node's descendants. The tree **s** could also be considered as a subtree of itself.

**Example 1:**
Given tree s:

```
     3
    / \
   4   5
  / \
 1   2
```

Given tree t:

```
   4 
  / \
 1   2
```

Return **true**, because t has the same structure and node values with a subtree of s.

+ Analysis
  + value match but not the same object
  + Note: node values are not unique
  + Works only for unique value nodes: breadth first traverse of first tree to find the starting node of the given tree. Then search the two trees in parallel
  + For values not unique: check if is subtree
    + if root value matches:
      + check if root in left, root in right, left in left and right in right

```python
    def isSubtree(self, s: TreeNode, t: TreeNode) -> bool:
        return self.traverse(s,t)
    def equals(self, x,y):
        if not x and not y: return True
        if not x or not y: return False
        return x.val==y.val and self.equals(x.left,y.left) and self.equals(x.right, y.right)
    def traverse(self,s,t):
        return s!=None and (self.equals(s,t) or self.traverse(s.left, t) or self.traverse(s.right,t))
```

