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

+ Stack:

  + Wish to multiply large number as late as possible:
    + Keep large number in stack, pop smaller number once seen
    + When we have 3 numbers, we are certain to remove the smallest one
    + When see a small number in middle, discard it and res+= the smaller product it forms with left/ right
    + resulting stack is a decreasing sequence
  + In the end pop all except first one
  + Example: [6,2,4, 5]
    + `[Max, 6]`
    + `[Max, 6, 2]`
    + `[Max, 6, 4] res+= 8 (2*4<2*8)`
    + `[Max, 6, 5] res +=  20 (4*5<4*6)`
    + `[Max, 6] res += 30 `
  + Example: [7, 12, 8, 10]
    + `[Max, 7]`
    + `[Max, 12] res+= 7*12`
    + `[Max, 12, 8]`
    + `[Max, 12, 10] res+=12*8`
    + `[Max, 12] res += 12*10`
  + Example: [8,12,7,10]
    + `[Max, 8]`
    + `[Max, 12] res+=8*12`
    + `[Max, 12, 7]`
    + `[Max, 12, 10] res+= 7*10 (7*10<7*12)`
    + `[Max, 12] res += 10`

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

  ```java
  class Solution {
      public int mctFromLeafValues(int[] arr) {
          int n = arr.length;
          Stack<Integer> stack = new Stack<>();
          int ans = 0;
          stack.push(Integer.MAX_VALUE);
          for (int cur: arr){
              while (stack.peek()<=cur){
                  int drop = stack.pop();
                  ans += drop*Math.min(stack.peek(), cur);
              }
              stack.push(cur);
          }
          
          while(stack.size()>2){
              ans += stack.pop()*stack.peek();
          }
          return ans;
      }
  }
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





### OA Find Maximum in a Tree

```python
import sys

def findMax(root):
    if not root: return 0
    curMax = -sys.maxsize-1
    stack = [root]
    while stack:
        cur = stack.pop()
        curMax = max(cur.val, curMax)
        if cur.left:
            stack.append(cur.left)
        if cur.right:
            stack.append(cur.right)
    return curMax
    
```

```Java
// Answer:    
static int findMax(TreeNode node)
    {
        if (node == null)
            return Integer.MIN_VALUE;
 
        int res = node.val;
        int lres = findMax(node.left);
        int rres = findMax(node.right);
 
        if (lres > res)
            res = lres;
        if (rres > res)
            res = rres;
        return res;
    }
```



### 108. Convert Sorted Array to Binary Search Tree

Easy

Given an array where elements are sorted in ascending order, convert it to a height balanced BST.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of *every* node never differ by more than 1.

**Example:**

```
Given the sorted array: [-10,-3,0,5,9],

One possible answer is: [0,-3,9,-10,null,5], which represents the following height balanced BST:

      0
     / \
   -3   9
   /   /
 -10  5
```

```python
    if not nums: return None
    if len(nums) == 1:
        return TreeNode(nums[0])
    midIdx = len(nums)//2
    node = TreeNode(nums[midIdx])
    node.left = self.sortedArrayToBST(nums[:midIdx])
    node.right = self.sortedArrayToBST(nums[midIdx+1:])
    return node
```





### 938. Range Sum of BST

Easy

Given the `root` node of a binary search tree, return *the sum of values of all nodes with a value in the range `[low, high]`*.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/11/05/bst1.jpg)

```
Input: root = [10,5,15,3,7,null,18], low = 7, high = 15
Output: 32
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/11/05/bst2.jpg)

```
Input: root = [10,5,15,3,7,13,18,1,null,6], low = 6, high = 10
Output: 23
```

 

```python
    def inOrder(node, res):
        if not node: return

        inOrder(node.left, res)
        if low<=node.val<=high:
            res[0] += node.val
        inOrder(node.right,res)

        return


    res = [0]        
    inOrder(root, res)

    return res[0]
```



### 669. Trim a Binary Search Tree

Medium

Given the `root` of a binary search tree and the lowest and highest boundaries as `low` and `high`, trim the tree so that all its elements lies in `[low, high]`. Trimming the tree should **not** change the relative structure of the elements that will remain in the tree (i.e., any node's descendant should remain a descendant). It can be proven that there is a **unique answer**.

Return *the root of the trimmed binary search tree*. Note that the root may change depending on the given bounds.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/09/09/trim1.jpg)

```
Input: root = [1,0,2], low = 1, high = 2
Output: [1,null,2]
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/09/09/trim2.jpg)

```
Input: root = [3,0,4,null,2,null,null,1], low = 1, high = 3
Output: [3,2,null,1]
```

**Example 3:**

```
Input: root = [1], low = 1, high = 2
Output: [1]
```

**Example 4:**

```
Input: root = [1,null,2], low = 1, high = 3
Output: [1,null,2]
```

**Example 5:**

```
Input: root = [1,null,2], low = 2, high = 4
Output: [2]
```

 ![image-20210130165428133](/home/arkyyang/files/notes/notes/attachments/image-20210130165428133.png)

```python
    def trim(node):
        if not node: return None
        if node.val>high: return trim(node.left)
        if node.val<low: return trim(node.right)
        node.left = trim(node.left)
        node.right = trim(node.right)
        return node
    return trim(root)
```

```java
class Solution {
    
    public TreeNode trimBST(TreeNode root, int low, int high) {
        if (root == null) return root;
        if (root.val > high) return trimBST(root.left, low, high);
        if (root.val < low) return trimBST(root.right, low, high);
        root.left = trimBST(root.left, low, high);
        root.right = trimBST(root.right, low, high);
        return root;
    }
}
```



### 449 Serialize and Deserialize BST

Medium

180192Add to ListShare

Serialization is converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a **binary search tree**. There is no restriction on how your serialization/deserialization algorithm should work. You need to ensure that a binary search tree can be serialized to a string, and this string can be deserialized to the original tree structure.

**The encoded string should be as compact as possible.**

 

**Example 1:**

```
Input: root = [2,1,3]
Output: [2,1,3]
```

**Example 2:**

```
Input: root = []
Output: []
```

 

+ Preorder:

```python
from collections import deque
class Codec:
    def serialize(self, root):
        res = []
        def preOrder(root):
            if root:
                res.append(root.val)
                preOrder(root.left)
                preOrder(root.right)
        preOrder(root)
        return ' '.join(map(str, res))


    def deserialize(self, data):
        data = deque(int(n) for n in data.split(' ') if n)
        def build(cmin, cmax):
            if data and cmin< data[0] < cmax:
                cur = data.popleft()
                node = TreeNode(cur)
                node.left = build(cmin, cur)
                node.right = build(cur, cmax)
                return node
        return build(float('-inf'), float('inf'))
```

+ Postoder allows tail recursion, faster:

```python
class Codec:
    def serialize(self, root):

        def postorder(root):
            return postorder(root.left) + postorder(root.right) + [root.val] if root else []
        return ' '.join(map(str, postorder(root)))

    def deserialize(self, data):

        def helper(lower = float('-inf'), upper = float('inf')):
            if not data or data[-1] < lower or data[-1] > upper:
                return None
            
            val = data.pop()
            root = TreeNode(val)
            root.right = helper(val, upper)
            root.left = helper(lower, val)
            return root
        
        data = [int(x) for x in data.split(' ') if x]
        return helper()
```

```java
public class Codec {
    
    StringBuilder postOrder(TreeNode root, StringBuilder sb){
        if (root == null)
            return sb;
        postOrder(root.left, sb);
        postOrder(root.right, sb);
        sb.append(root.val);
        sb.append(' ');
        return sb;
    }

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        StringBuilder sb = postOrder(root, new StringBuilder());
        if (sb.length() > 0)
            sb.deleteCharAt(sb.length() -1);
        return sb.toString();
    }
    
    TreeNode helper(Integer lower, Integer upper, ArrayDeque<Integer> nums){
        if (nums.isEmpty())
            return null;
        int val = nums.getLast();
        if (val<lower || val > upper)
            return null;
        nums.removeLast();
        TreeNode root  = new TreeNode(val);
        root.right = helper(val, upper, nums);
        root.left = helper(lower, val, nums);
        return root;
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        if (data.isEmpty())
            return null;
        ArrayDeque<Integer> nums = new ArrayDeque<Integer>();
        for (String s: data.split("\\s+"))
            nums.add(Integer.valueOf(s));
        return helper(Integer.MIN_VALUE, Integer.MAX_VALUE, nums);
        
    }
}
```



### 426. Convert Binary Search Tree to Sorted Doubly Linked List

Medium

Convert a **Binary Search Tree** to a sorted **Circular Doubly-Linked List** in place.

You can think of the left and right pointers as synonymous to the predecessor and successor pointers in a doubly-linked list. For a circular doubly linked list, the predecessor of the first element is the last element, and the successor of the last element is the first element.

We want to do the transformation **in place**. After the transformation, the left pointer of the tree node should point to its predecessor, and the right pointer should point to its successor. You should return the pointer to the smallest element of the linked list.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2018/10/12/bstdlloriginalbst.png)

```
Input: root = [4,2,5,1,3]


Output: [1,2,3,4,5]

Explanation: The figure below shows the transformed BST. The solid line indicates the successor relationship, while the dashed line means the predecessor relationship.
```

**Example 2:**

```
Input: root = [2,1,3]
Output: [1,2,3]
```

**Example 3:**

```
Input: root = []
Output: []
Explanation: Input is an empty tree. Output is also an empty Linked List.
```

**Example 4:**

```
Input: root = [1]
Output: [1]
```

 

```python
class Solution:
    def treeToDoublyList(self, root: 'Node') -> 'Node':
        if not root: return None
        
        def inOrder(root):
            if not root.left:
                first = root
            else:
                first, leftLast = inOrder(root.left)
                root.left = leftLast
                leftLast.right = root
            if not root.right:
                last = root
            else:
                rightFirst, last = inOrder(root.right)
                rightFirst.left = root
                root.right = rightFirst
            
            return first, last
        
        first, last = inOrder(root)
        first.left = last
        last.right = first
        return first
```



### 99. Recover Binary Search Tree

Hard

You are given the `root` of a binary search tree (BST), where exactly two nodes of the tree were swapped by mistake. *Recover the tree without changing its structure*.

**Follow up:** A solution using `O(n)` space is pretty straight forward. Could you devise a constant space solution?

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/10/28/recover1.jpg)

```
Input: root = [1,3,null,null,2]
Output: [3,1,null,null,2]
Explanation: 3 cannot be a left child of 1 because 3 > 1. Swapping 1 and 3 makes the BST valid.
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/10/28/recover2.jpg)

```
Input: root = [3,1,4,null,null,2]
Output: [2,1,4,null,null,3]
Explanation: 2 cannot be in the right subtree of 3 because 2 < 3. Swapping 2 and 3 makes the BST valid.
```

 

+ 


+ Recursion: technically $O(n)$ memory worst

```python
def inorder(root):
    nonlocal first, second, prev
    if not root: return
    inorder(root.left)
    if prev and prev.val > root.val:
        if not first:
            first = prev
        second = root
    prev = root
    inorder(root.right)

first = second = prev = None
inorder(root)
first.val, second.val = second.val, first.val
```



+ Morris Traversal

```python
class Solution:
    def recoverTree(self, root):
        """
        :type root: TreeNode
        :rtype: void Do not return anything, modify root in-place instead.
        """

        x = y = predecessor = pred = None
        
        while root:

            if root.left:       
                predecessor = root.left
                while predecessor.right and predecessor.right != root:
                    predecessor = predecessor.right

                if predecessor.right is None:
                    predecessor.right = root
                    root = root.left

                else:

                    if pred and root.val < pred.val:
                        y = root
                        if x is None:
                            x = pred 
                    pred = root
                    
                    predecessor.right = None
                    root = root.right

            else:

                if pred and root.val < pred.val:
                    y = root
                    if x is None:
                        x = pred 
                pred = root
                
                root = root.right
        
        x.val, y.val = y.val, x.val
```



### 1339. Maximum Product of Splitted Binary Tree

Medium

Given a binary tree `root`. Split the binary tree into two subtrees by removing 1 edge such that the product of the sums of the subtrees are maximized.

Since the answer may be too large, return it modulo 10^9 + 7.

 

**Example 1:**

**![img](https://assets.leetcode.com/uploads/2020/01/21/sample_1_1699.png)**

```
Input: root = [1,2,3,4,5,6]
Output: 110
Explanation: Remove the red edge and get 2 binary trees with sum 11 and 10. Their product is 110 (11*10)
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/01/21/sample_2_1699.png)

```
Input: root = [1,null,2,3,4,null,null,5,6]
Output: 90
Explanation:  Remove the red edge and get 2 binary trees with sum 15 and 6.Their product is 90 (15*6)
```

**Example 3:**

```
Input: root = [2,3,9,10,7,8,6,5,4,11,1]
Output: 1025
```

**Example 4:**

```
Input: root = [1,1]
Output: 1
```

 

**Constraints:**

- Each tree has at most `50000` nodes and at least `2` nodes.
- Each node's value is between `[1, 10000]`.



+ We need at each node, we need to find to sums: 
  + sum of its subtree including itself, sum of all other nodes

```python
class Solution:
    def maxProduct(self, root: TreeNode) -> int:
        def findSum(root):
            return root.val + findSum(root.left) + findSum(root.right) if root else 0
        
        tot = findSum(root)
        
        def findCut(node, maxProd):
            if not node: return 0
            curSum = node.val + findCut(node.left, maxProd) + findCut(node.right, maxProd)
            maxProd[0] = max(maxProd[0], curSum*(tot-curSum))
            return curSum
        
        maxProd = [0]
        findCut(root, maxProd)
        return maxProd[0]%(10**9 + 7)
```

```java
class Solution {
    public int maxProduct(TreeNode root) {
        int tot = findSum(root);
        long[] maxProd = {0};
        findCut(root, maxProd, tot);
        return (int) (maxProd[0] % 1000000007);
    }
    
    int findSum(TreeNode root){
        return (root==null)? 0: root.val + findSum(root.left) + findSum(root.right);
    }
    
    int findCut(TreeNode node, long[] maxProd, long tot){
        if (node == null)
            return 0;
        int curSum = node.val + findCut(node.left, maxProd, tot) + findCut(node.right, maxProd, tot);
        maxProd[0]  = Math.max(maxProd[0], (long)curSum*(tot - curSum));
        return curSum;
    }
}
```



### 1676. Lowest Common Ancestor of a Binary Tree IV

Medium

Given the `root` of a binary tree and an array of `TreeNode` objects `nodes`, return *the lowest common ancestor (LCA) of **all the nodes** in* `nodes`. All the nodes will exist in the tree, and all values of the tree's nodes are **unique**.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], nodes = [4,7]
Output: 2
Explanation: The lowest common ancestor of nodes 4 and 7 is node 2.
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], nodes = [1]
Output: 1
Explanation: The lowest common ancestor of a single node is the node itself.
```

**Example 3:**

![img](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], nodes = [7,6,2,4]
Output: 5
Explanation: The lowest common ancestor of the nodes 7, 6, 2, and 4 is node 5.
```

**Example 4:**

![img](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], nodes = [0,1,2,3,4,5,6,7,8]
Output: 3
Explanation: The lowest common ancestor of all the nodes is the root node.
```

 

+ If in both left and right and visited all:
  + return node
+ if only in left / right:
  + return node.left/ right

+ See if tree contains nodes:

```python
def contains(root, nodes):
    if not root: return None
    if root in nodes: return True
    return contains(root.left) or contains(root.right)
```

+ Modify to common ancestor

```python
def ancestor(root, nodes):
    if not root: return None
    if root in nodes: return node
    left, right = ancestor(root.left, nodes), ancestor(root.right, nodes)
    if not left:
        return right
    if not right:
        return left
    else:
        return root
```

```java
class Solution {
    Set<Integer> set;
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode[] nodes) {
        set = new HashSet<>();
        for (TreeNode t: nodes){
            set.add(t.val);
        }
        return find(root);
    }
    
    TreeNode find(TreeNode node){
        if (node==null) 
            return null;
        if (set.contains(node.val))
            return node;
        TreeNode left = find(node.left);
        TreeNode right = find(node.right);
        if (left==null){
            return right;
        }else if(right == null){
            return left;
        }else{
            return node;
        }
    }
}
```



### 129. Sum Root to Leaf Numbers

Medium

Given a binary tree containing digits from `0-9` only, each root-to-leaf path could represent a number.

An example is the root-to-leaf path `1->2->3` which represents the number `123`.

Find the total sum of all root-to-leaf numbers.

**Note:** A leaf is a node with no children.

**Example:**

```
Input: [1,2,3]
    1
   / \
  2   3
Output: 25
Explanation:
The root-to-leaf path 1->2 represents the number 12.
The root-to-leaf path 1->3 represents the number 13.
Therefore, sum = 12 + 13 = 25.
```

**Example 2:**

```
Input: [4,9,0,5,1]
    4
   / \
  9   0
 / \
5   1
Output: 1026
Explanation:
The root-to-leaf path 4->9->5 represents the number 495.
The root-to-leaf path 4->9->1 represents the number 491.
The root-to-leaf path 4->0 represents the number 40.
Therefore, sum = 495 + 491 + 40 = 1026.
```

```python
class Solution:
    def sumNumbers(self, root: TreeNode) -> int:
        def findSum(node, path):
            if not node: return 0
            cur  = path*10 + node.val
            if not node.left and not node.right:
                return cur
            return findSum(node.left, cur) + findSum(node.right,cur)
        
        return findSum(root, 0)
```

```java
class Solution {
    public int sumNumbers(TreeNode root) {
        return findSum(root, 0);
    }
    
    int findSum(TreeNode node, int path){
        if (node == null)
            return 0;
        int cur = path*10 + node.val;
        if (node.left==null && node.right == null){
            return cur;
        }
        return findSum(node.left, cur) + findSum(node.right, cur);
    }
}
```



### 114. Flatten Binary Tree to Linked List

Medium

Given the `root` of a binary tree, flatten the tree into a "linked list":

- The "linked list" should use the same `TreeNode` class where the `right` child pointer points to the next node in the list and the `left` child pointer is always `null`.
- The "linked list" should be in the same order as a [**pre-order** **traversal**](https://en.wikipedia.org/wiki/Tree_traversal#Pre-order,_NLR) of the binary tree.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/01/14/flaten.jpg)

```
Input: root = [1,2,5,3,4,null,6]
Output: [1,null,2,null,3,null,4,null,5,null,6]
```

**Example 2:**

```
Input: root = []
Output: []
```

**Example 3:**

```
Input: root = [0]
Output: [0]
```

 

```python
class Solution:
    def flatten(self, root: TreeNode) -> None:
        if not root: return root
        pre = None
        cur = root
        stack = [root]
        while stack:
            if pre:
                pre.right, pre.left = cur, None
            if cur.right: stack.append(cur.right)
            if cur.left:
                pre, cur = cur, cur.left
            else:
                pre, cur = cur, stack.pop()
            
        return
```

```java
class Solution {
    public void flatten(TreeNode root) {
        if (root==null)
            return;
        TreeNode pre = null;
        TreeNode cur = root;
        Stack<TreeNode> stack = new Stack<TreeNode>();
        stack.push(root);
        while(!stack.isEmpty()){
            if(pre!=null){
                pre.right = cur;
                pre.left = null;
            }
            if(cur.right!=null)
                stack.push(cur.right);
            if(cur.left!=null){
                pre = cur;
                cur = cur.left;
            }else{
                pre = cur;
                cur = stack.pop();
            }
        }
    return;
    }
}
```



### 889. Construct Binary Tree from Preorder and Postorder Traversal

Medium

Return any binary tree that matches the given preorder and postorder traversals.

Values in the traversals `pre` and `post` are distinct positive integers.

 

**Example 1:**

```
Input: pre = [1,2,4,5,3,6,7], post = [4,5,2,6,7,3,1]
Output: [1,2,3,4,5,6,7]
```

 

**Note:**

- `1 <= pre.length == post.length <= 30`
- `pre[]` and `post[]` are both permutations of `1, 2, ..., pre.length`.
- It is guaranteed an answer exists. If there exists multiple answers, you can return any of them.

+ **Be careful when finding relative position, use offset like `idx-postl` instead of absolute `idx`, except for the single `idx+1`**

```python
class Solution:
    def constructFromPrePost(self, pre: List[int], post: List[int]) -> TreeNode:

        def build(prel, prer, postl, postr):
            print(prel, prer, postl, postr)
            if prel>prer or postl>postr: return None
            if prel==prer: return TreeNode(pre[prel])
            val = pre[prel]
            node = TreeNode(val)
            for i in range(postl, postr+1):
                if post[i] == pre[prel+1]:
                    idx = i
            prel += 1
            node.left = build(prel, prel+idx-postl, postl, idx)
            node.right = build(prel+idx-postl+1, prer,idx+1, postr-1)
            return node
        
        n = len(pre)
        return build(0, n-1, 0, n-1)
```

