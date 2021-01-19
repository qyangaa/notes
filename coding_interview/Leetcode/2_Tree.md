### 515. Find Largest Value in Each Tree Row

Medium

Given the `root` of a binary tree, return *an array of the largest value in each row* of the tree **(0-indexed)**.

 

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/08/21/largest_e1.jpg)

```
Input: root = [1,3,2,5,3,null,9]
Output: [1,3,9]
```

**Example 2:**

```
Input: root = [1,2,3]
Output: [1,3]
```

**Example 3:**

```
Input: root = [1]
Output: [1]
```

**Example 4:**

```
Input: root = [1,null,2]
Output: [1,2]
```

**Example 5:**

```
Input: root = []
Output: []
```



+ BFS, remember each row

```python
        if not root: return []
        maxArr=[]
        queue = deque([root])
        
        
        while queue:
            curMax = -sys.maxsize-1
            queue2 = deque([])
            while queue:
                cur = queue.popleft()
                curMax = max(curMax, cur.val)
                if cur.left: queue2.append(cur.left)
                if cur.right: queue2.append(cur.right)
            queue = queue2
            maxArr.append(curMax)
        return maxArr
```



### 199. Binary Tree Right Side View

Medium

3208171Add to ListShare

Given a binary tree, imagine yourself standing on the *right* side of it, return the values of the nodes you can see ordered from top to bottom.

**Example:**

```
Input: [1,2,3,null,5,null,4]
Output: [1, 3, 4]
Explanation:

   1            <---
 /   \
2     3         <---
 \     \
  5     4       <---
```

+ BFS remember level, output last at each level

```python
from collections import deque

if not root: return []
view = []
queue = deque([root])
while queue:
    queue2=deque()
    lastVal = None
    while queue:
        cur = queue.popleft()
        lastVal = cur.val
        if cur.left: queue2.append(cur.left)
        if cur.right: queue2.append(cur.right)
    queue = queue2
    view.append(lastVal)
return view
```



### 110. Balanced Binary Tree

Easy

Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as:

> a binary tree in which the left and right subtrees of *every* node differ in height by no more than 1.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/10/06/balance_1.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: true
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/10/06/balance_2.jpg)

```
Input: root = [1,2,2,3,3,null,null,4,4]
Output: false
```

**Example 3:**

```
Input: root = []
Output: true
```



+ Recursion on depth

```python
    if not root: return True

    def getHeight(root):
        if not root: return -1

        leftHeight = getHeight(root.left)
        if leftHeight==-2: return -2
        rightHeight = getHeight(root.right)
        if rightHeight==-2: return -2
        if abs(leftHeight-rightHeight)>1:
            return -2
        return max(leftHeight, rightHeight )+1

    return getHeight(root)>-2
```

### 257. Binary Tree Paths

Easy

Given a binary tree, return all root-to-leaf paths.

**Note:** A leaf is a node with no children.

**Example:**

```
Input:

   1
 /   \
2     3
 \
  5

Output: ["1->2->5", "1->3"]

Explanation: All root-to-leaf paths are: 1->2->5, 1->3
```

```python
    if not root: return []

    def findPath(root, path, res):
        if not root.left and not root.right: 
            res.append(path+"->"+str(root.val))
            return
        if root.left:
            findPath(root.left,path+"->"+str(root.val),res)
        if root.right:
            findPath(root.right,path+"->"+str(root.val),res)
        return

    path = str(root.val)
    res=[]
    if not root.left and not root.right:
        res.append(path)
    if root.left:
        findPath(root.left,path,res)
    if root.right:
        findPath(root.right,path,res)
    return res
```



### 987. Vertical Order Traversal of a Binary Tree

Medium

Given the `root` of a binary tree, calculate the **vertical order traversal** of the binary tree.

For each node at position `(x, y)`, its left and right children will be at positions `(x - 1, y - 1)` and `(x + 1, y - 1)` respectively.

The **vertical order traversal** of a binary tree is a list of non-empty **reports** for each unique x-coordinate from left to right. Each **report** is a list of all nodes at a given x-coordinate. The **report** should be primarily sorted by y-coordinate from highest y-coordinate to lowest. If any two nodes have the same y-coordinate in the **report**, the node with the smaller value should appear earlier.

Return *the **vertical order traversal** of the binary tree*.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2019/01/31/1236_example_1.PNG)

```
Input: root = [3,9,20,null,null,15,7]
Output: [[9],[3,15],[20],[7]]
Explanation: Without loss of generality, we can assume the root node is at position (0, 0):
The node with value 9 occurs at position (-1, -1).
The nodes with values 3 and 15 occur at positions (0, 0) and (0, -2).
The node with value 20 occurs at position (1, -1).
The node with value 7 occurs at position (2, -2).
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2019/01/31/tree2.png)

```
Input: root = [1,2,3,4,5,6,7]
Output: [[4],[2],[1,5,6],[3],[7]]
Explanation: The node with value 5 and the node with value 6 have the same position according to the given scheme.
However, in the report [1,5,6], the node with value 5 comes first since 5 is smaller than 6.
```

```python
from collections import deque
import bisect
class Solution:
    def verticalTraversal(self, root: TreeNode) -> List[List[int]]:
        if not root: return []
        res = deque([])
        queue = deque([[root, 0]])
        rootIdx = 0
        while queue:
            cur, curIdx = queue.popleft()
            realIdx = curIdx+rootIdx
            if realIdx>=len(res):
                res.append([cur.val])
            elif realIdx<0:
                res.appendleft([cur.val])
                rootIdx+=1
            else:
                # bisect.insort_left(res[realIdx],cur.val) # if need to store in order
                res[realIdx].append(cur.val)
                
            
            if cur.left: queue.append([cur.left, curIdx-1])
            if cur.right: queue.append([cur.right, curIdx+1])  
        return list(res)
```



### 173. Binary Search Tree Iterator

Medium

Implement the `BSTIterator` class that represents an iterator over the **[in-order traversal](https://en.wikipedia.org/wiki/Tree_traversal#In-order_(LNR))** of a binary search tree (BST):

- `BSTIterator(TreeNode root)` Initializes an object of the `BSTIterator` class. The `root` of the BST is given as part of the constructor. The pointer should be initialized to a non-existent number smaller than any element in the BST.
- `boolean hasNext()` Returns `true` if there exists a number in the traversal to the right of the pointer, otherwise returns `false`.
- `int next()` Moves the pointer to the right, then returns the number at the pointer.

Notice that by initializing the pointer to a non-existent smallest number, the first call to `next()` will return the smallest element in the BST.

You may assume that `next()` calls will always be valid. That is, there will be at least a next number in the in-order traversal when `next()` is called.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2018/12/25/bst-tree.png)

```
Input
["BSTIterator", "next", "next", "hasNext", "next", "hasNext", "next", "hasNext", "next", "hasNext"]
[[[7, 3, 15, null, null, 9, 20]], [], [], [], [], [], [], [], [], []]
Output
[null, 3, 7, true, 9, true, 15, true, 20, false]

Explanation
BSTIterator bSTIterator = new BSTIterator([7, 3, 15, null, null, 9, 20]);
bSTIterator.next();    // return 3
bSTIterator.next();    // return 7
bSTIterator.hasNext(); // return True
bSTIterator.next();    // return 9
bSTIterator.hasNext(); // return True
bSTIterator.next();    // return 15
bSTIterator.hasNext(); // return True
bSTIterator.next();    // return 20
bSTIterator.hasNext(); // return False
```

 ```python
class BSTIterator:

    def __init__(self, root: TreeNode):
        self.list = [None]
        self.curIdx = 0
        self.__inOrder__(root)
    
    def __inOrder__(self, root):
        if not root: return
        if root.left:
            self.__inOrder__(root.left)
        self.list.append(root.val)
        if root.right:
            self.__inOrder__(root.right)
        return 
        

    def next(self) -> int:
        self.curIdx+=1
        return self.list[self.curIdx]
        

    def hasNext(self) -> bool:
        return self.curIdx<len(self.list)-1
 ```

+ Controlled traversal, not faster but interesting

  ```python
  class BSTIterator:
  
      def __init__(self, root: TreeNode):
  
          self.stack = []
          self._leftmost_inorder(root)
  
      def _leftmost_inorder(self, root):
  
          while root:
              self.stack.append(root)
              root = root.left
  
      def next(self) -> int:
  
          topmost_node = self.stack.pop()
          if topmost_node.right:
              self._leftmost_inorder(topmost_node.right)
          return topmost_node.val
  
      def hasNext(self) -> bool:
          return len(self.stack) > 0
          
  ```

  

### 572. Subtree of Another Tree

Easy

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

 

**Example 2:**
Given tree s:

```
     3
    / \
   4   5
  / \
 1   2
    /
   0
```

Given tree t:

```
   4
  / \
 1   2
```

Return **false**.





```python
from collections import deque
class Solution:
    def isSubtree(self, s: TreeNode, t: TreeNode) -> bool:
        def markHeight(root):
            if not root: return
            if not root.left and not root.right:
                root.h = 0
                return
            if not root.left:
                markHeight(root.right)
                root.h = root.right.h+1
                return
            if not root.right:
                markHeight(root.left)
                root.h = root.left.h+1
                return               
            markHeight(root.left)
            markHeight(root.right)
            root.h = max(root.left.h,root.right.h)+1
            return
        
        def checkSame(s,t):
            if not s and not t: return True
            if not s or not t: return False
            if s.val!=t.val: return False
            return checkSame(s.left, t.left) and checkSame(s.right, t.right)
        
        markHeight(s)
        markHeight(t)
        stack=[s]
        while stack:
            cur = stack.pop()
            if cur.h==t.h and cur.val==t.val:
                if checkSame(cur,t): return True
                continue
            if cur.left: stack.append(cur.left)
            if cur.right: stack.append(cur.right)
        return False
```

+ Easier method is preorder traversal and remembering left and right null nodes



### 1130. Minimum Cost Tree From Leaf Values

Medium

Given an array `arr` of positive integers, consider all binary trees such that:

- Each node has either 0 or 2 children;
- The values of `arr` correspond to the values of each **leaf** in an in-order traversal of the tree. *(Recall that a node is a leaf if and only if it has 0 children.)*
- The value of each non-leaf node is equal to the product of the largest leaf value in its left and right subtree respectively.

Among all possible binary trees considered, return the smallest possible sum of the values of each non-leaf node. It is guaranteed this sum fits into a 32-bit integer.

 

**Example 1:**

```
Input: arr = [6,2,4]
Output: 32
Explanation:
There are two possible trees.  The first has non-leaf node sum 36, and the second has non-leaf node sum 32.

    24            24
   /  \          /  \
  12   4        6    8
 /  \               / \
6    2             2   4
```



+ Recursion:
  + If one node: add 0
  + if two node: add a*b
  + if three node: add `min(a*max(b,c), b*max(a,c), c*max(a,b))`
  + n node: spit into left right
    + `sum = recurse(left)+ recurse(right)+max(left)*max(right)`

+ Time limit exceeded:

  ```python
  import sys
  class Solution:
      def mctFromLeafValues(self, arr: List[int]) -> int:
          def helper(start, end):
              if start>=end: return 0
              if start==end-1: return arr[start]*arr[start+1]
              minSum = sys.maxsize
              for i in range(start, end):
                  curSum = helper(start,i)+helper(i+1,end)+max(arr[start:i+1])*max(arr[i+1:end+1])
                  minSum = min(minSum, curSum)
              return minSum
          
          
          n = len(arr)
          return helper(0,n-1)
  ```

+ Greedy solution

  + Pick up the leaf node with minimum value.
  + Combine it with its inorder neighbor which has smaller value between neighbors.
  + Once we get the new generated non-leaf node, the node with minimum value is useless (For the new generated subtree will be represented with the largest leaf node value.)
  + Repeat it until there is only one node.

  ```python
  res = 0
  while len(arr)>1:
      min_idx = arr.index(min(arr))
      if 0<min_idx<len(arr)-1:
          res += min(arr[min_idx-1], arr[min_idx+1])*arr[min_idx]
      else:
          res+=arr[1 if min_idx==0 else min_idx-1]*arr[min_idx]
      arr.pop(min_idx)
  return res
  ```

  

### 545. Boundary of Binary Tree

Medium

A binary tree boundary is the set of nodes (**no duplicates**) denoting the union of the **left boundary**, **leaves**, and **right boundary**.

The **left boundary** is the set of nodes on the path from the root to the **left-most** node. The **right boundary** is the set of nodes on the path from the root to the **right-most** node.

The **left-most** node is the **leaf** node you reach when you always travel to the left subtree if it exists and the right subtree if it doesn't. The **right-most** node is defined in the same way except with left and right exchanged. Note that the root may be the **left-most** and/or **right-most** node.

Given the `root` of a binary tree, return *the values of its **boundary** in a **counter-clockwise** direction starting from the* `root`.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/11/11/boundary1.jpg)

```
Input: root = [1,null,2,3,4]
Output: [1,3,4,2]
Explanation:
The left boundary is the nodes [1,2,3].
The right boundary is the nodes [1,2,4].
The leaves are nodes [3,4].
Unioning the sets together gives [1,2,3,4], which is [1,3,4,2] in counter-clockwise order.
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/11/11/boundary2.jpg)

```
Input: root = [1,2,3,4,5,6,null,null,null,7,8,9,10]
Output: [1,2,4,7,8,9,10,6,3]
Explanation:
The left boundary are node 1,2,4. (4 is the left-most node according to definition)
The left boundary is nodes [1,2,4].
The right boundary is nodes [1,3,6,10].
The leaves are nodes [4,7,8,9,10].
Unioning the sets together gives [1,2,3,4,6,7,8,9,10], which is [1,2,4,7,8,9,10,6,3] in counter-clockwise order.
```