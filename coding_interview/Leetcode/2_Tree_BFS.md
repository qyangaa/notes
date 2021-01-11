### 102 Binary Tree Level Order Traversal

Given a binary tree, return the *level order* traversal of its nodes' values. (ie, from left to right, level by level).

+ Example

  + Given binary tree `[3,9,20,null,null,15,7]`,

    ```
        3
       / \
      9  20
        /  \
       15   7
    ```

    

    return its level order traversal as:

    ```
    [
      [3],
      [9,20],
      [15,7]
    ]
    ```

+ Analysis: 

  + every level in its own list, pre-order (put current level in first)

  + Recursion:

    ```python
        def levelOrder(self, root) -> int:
            if not root: return []
            self.mainList=[]
            self.getLevel(root,0)
            return self.mainList
    
        def getLevel(self,root, curIdx):
            if curIdx == len(self.mainList):
                self.mainList += [[root.val]]
            else:
                self.mainList[curIdx].append(root.val)
            nextIdx=curIdx+1
            if root.left:
                self.getLevel(root.left, nextIdx)
            if root.right:
                self.getLevel(root.right, nextIdx)
    
    ```

  + Using queue is slightly faster:

  ```python
  from collections import deque
  
  def levelOrder(self, root):
      if not root: return []
      queue, res=deque([root]),[]
      while queue:
          cur_level, size = [], len(queue)
          for i in range(size):
              node=queue.popleft()
              if node.left:
                  queue.append(node.left)
              if node.right:
                  queue.append(node.right)
              cur_level.append(node.val)
          res.append(cur_level)
      return res
  ```

### 103 Binary Tree Zigzag Level Order Traversal

from left to right, then right to left for the next level and alternate between).

+ Example

  + For example:
    Given binary tree `[3,9,20,null,null,15,7]`,

    ```
        3
       / \
      9  20
        /  \
       15   7
    ```

    

    return its zigzag level order traversal as:

    ```
    [
      [3],
      [20,9],
      [15,7]
    ]
    ```

  + Note: we invert the whole level rather than each pair

+ queue

  ```python
  def zigzagLevelOrder(self, root):
      if not root: return []
      queue, res = deque([root]), []
      while queue:
          curLevel, size, level = [], len(queue), len(res)
          for i in range(size):
              node = queue.popleft()
              if node.left:
                  queue.append(node.left)
              if node.right:
                  queue.append(node.right)
              if level%2==1:
              	curLevel.append(node.val)
              else:
                  curLevel.insert(0,node.val)
          res.append(curLevel)
      return res
  ```

+ Use simple list rather than queue:

  ```python
  nodes, ans = [root], []
  while nodes := [n for n in nodes if n]:
  	level = [n.val for n in nodes]
  	if len(ans) % 2 == 1:
  		level.reverse()
  	ans.append(level)
  	nodes = [c for p in nodes for c in (p.left, p.right)]
  return ans
  ```

  

### 107 Binary Tree Level Order Traversal II

Given a binary tree, return the *bottom-up level order* traversal of its nodes' values. (ie, from left to right, level by level from leaf to root).

+ Example:
  Given binary tree `[3,9,20,null,null,15,7]`,

  ```
      3
     / \
    9  20
      /  \
     15   7
  ```

  

  return its bottom-up level order traversal as:

  ```
  [
    [15,7],
    [9,20],
    [3]
  ]
  ```

+ Insert front for each list

  ```python
  curLevel, ans=[root],[]
  while curLevel := [n for n in curLevel if n]:
      level=[n.val for n in curLevel]
      ans.insert(0,level)
      curLevel=[c for p in curLevel for c in (p.left, p.right)]
  return ans
  ```

+ Tip: use deque and appendleft is faster than array insert:

  ```python
  class Solution:
      def levelOrderBottom(self, root: TreeNode) -> List[List[int]]:
          q, r = [root] if root else [], collections.deque([])
          while q:
              r.appendleft([n.val for n in q])
              q = [c for n in q for c in [n.left, n.right] if c]
          return list(r)
  ```

  

### 111 Minimum Depth of Binary Tree

Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

**Note:** A leaf is a node with no children.

+ Example

  + **Example 1:**

    ![img](https://assets.leetcode.com/uploads/2020/10/12/ex_depth.jpg)

    ```
    Input: root = [3,9,20,null,null,15,7]
    Output: 2
    ```

    **Example 2:**

    ```
    Input: root = [2,null,3,null,4,null,5,null,6]
    Output: 5
    ```

+ Analysis:

  + pre-order traversal, return level once see a leaf

  + ```python
    class Solution:
        def minDepth(self, root: TreeNode) -> int:
            if not root: return 0
            curLevel=[root]
            level=1
            while len(curLevel)>0:
                nextLevel=[]
                for node in curLevel:
                    if not node.left and not node.right:
                        return level
                    if node.left:
                        nextLevel.append(node.left)
                    if node.right:
                        nextLevel.append(node.right)
                curLevel=nextLevel
                level+=1
            return level
        
    ```

### 116 Populating Next Right Pointers in Each Node

You are given a **perfect binary tree** where all leaves are on the same level, and every parent has two children. 

![Determine the depth and number of node in perfect binary tree if  index-number given - Mathematics Stack Exchange](data:image/png;base64,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)

The binary tree has the following definition:

following definition:

```
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```

Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to `NULL`.

Initially, all next pointers are set to `NULL`.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2019/02/14/116_sample.png)

```
Input: root = [1,2,3,4,5,6,7]
Output: [1,#,2,3,#,4,5,6,7,#]
Explanation: Given the above perfect binary tree (Figure A), your function should populate each next pointer to point to its next right node, just like in Figure B. The serialized output is in level order as connected by the next pointers, with '#' signifying the end of each level.
```

```python
class Solution:
    def connect(self, root: 'Node') -> 'Node':
        if not root: return root
        curLevel=[root]
        while len(curLevel)>0:
            nextLevel=[]
            curLevel.append(None)
            for i in range(len(curLevel)-1):
                curLevel[i].next=curLevel[i+1]
                if curLevel[i].left:
                    nextLevel.append(curLevel[i].left)
                    nextLevel.append(curLevel[i].right)
            curLevel=nextLevel
        return root
```



### 117 Populating Next Right Pointers in Each Node II

Given a Normal binary tree

**Example 1:**

![img](https://assets.leetcode.com/uploads/2019/02/15/117_sample.png)

```
Input: root = [1,2,3,4,5,null,7]
Output: [1,#,2,3,#,4,5,7,#]
Explanation: Given the above binary tree (Figure A), your function should populate each next pointer to point to its next right node, just like in Figure B. The serialized output is in level order as connected by the next pointers, with '#' signifying the end of each level.
```

```python
class Solution:
    def connect(self, root: 'Node') -> 'Node':
        if not root: return root
        curLevel=[root]
        while len(curLevel)>0:
            nextLevel=[]
            curLevel.append(None)
            for i in range(len(curLevel)-1):
                curLevel[i].next=curLevel[i+1]
                if curLevel[i].left:
                    nextLevel.append(curLevel[i].left)
                if curLevel[i].right:
                    nextLevel.append(curLevel[i].right)
            curLevel=nextLevel
        return root
```



### 199 Binary Tree Right Side View

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

```python
class Solution:
    def rightSideView(self, root: TreeNode) -> List[int]:
        if not root: return root
        curLevel=[root]
        res=[]
        while len(curLevel)>0:
            nextLevel=[]
            for node in curLevel:
                if node.left:
                    nextLevel.append(node.left)
                if node.right:
                    nextLevel.append(node.right)
            res.append(curLevel[-1].val)
            curLevel=nextLevel
         return res
                    
        
```



### 200 Number of Islands

Given an `m x n` 2d `grid` map of `'1'`s (land) and `'0'`s (water), return *the number of islands*.

An **island** is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

 

**Example 1:**

```
Input: grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
Output: 1
```

**Example 2:**

```
Input: grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
Output: 3
```

+ Analysis
  + definitions:
    + legalStep: step to 1, not out of boundary, not visited
    + terminate: if no legalSteps anymore
  + BFS
    + Start from `[0,0]`, go until terminate, count++
    + Find an unvisited 1, go until terminate, count++ for each iteration

```python
    def numIslands(self, grid) -> int:
        self.m = len(grid)
        self.n = len(grid[0])
        self.grid = grid
        self.toVisit = set([(i, j) for i in range(self.m) for j in range(self.n) if self.grid[i][j] == "1"])
        num_islands=0
        while len(self.toVisit)>0:
            start=self.toVisit.pop()
            self.BFS(start)
            num_islands+=1
        return num_islands

    def BFS(self, start):
        curLevel=[start]
        while len(curLevel)>0:
            nextLevel=[]
            for item in curLevel:
                nextLevel+=self.findLegalSteps(item)
            curLevel=nextLevel
        return

    def findLegalSteps(self, item):
        legalSteps=[]
        i,j=item[0],item[1]
        for step in [(i+1,j),(i-1,j),(i,j+1),(i,j-1)]:
            if self.isLegal(step[0],step[1]):
                legalSteps.append(step)
                self.toVisit.remove(step)
        return legalSteps

    def isLegal(self, i, j):
        if not (i, j) in self.toVisit:
            return False
        if i < 0 or j < 0 or i >= self.m or j >= self.n:
            return False
        return True
```

+ DFS solution can be faster: (176ms vs 224ms, not too much difference)

  + mark visited using the original grid (set to "0")

  + ```python
    def numIslands(self, grid):
        if not grid:
            return 0
            
        m = len(grid)
        n = len(grid[0])
        sum  = 0
        
        #linear scanning doesn't take too much time. Ok to do it rather than remembering places to visit
        for i in range(m):
            for j in range(n):
                if grid[i][j] == "0":
                    continue
                else:
                    #sum up only once per chance of meeting "1"
                    sum += 1
                    stack = list()
                    stack.append([i,j])
                    #visit each "1" in the adjacent area using a stack
                    while len(stack) != 0:
                        [p,q] = stack.pop()
                        if p >= 1 and grid[p-1][q] == "1":
                            stack.append([p-1,q])
                        if p < m -1 and grid[p+1][q] == "1":
                            stack.append([p+1,q])
                        if q >= 1 and grid[p][q-1] == "1":
                            stack.append([p,q-1])  
                        if q < n - 1 and grid[p][q + 1] == "1":
                            stack.append([p,q+1])
                        #mark as visited
                        grid[p][q] = "0"
        
        return sum
    ```



### 637 Average of Levels in Binary Tree

Given a non-empty binary tree, return the average value of the nodes on each level in the form of an array.

**Example 1:**

```
Input:
    3
   / \
  9  20
    /  \
   15   7
Output: [3, 14.5, 11]
Explanation:
The average value of nodes on level 0 is 3,  on level 1 is 14.5, and on level 2 is 11. Hence return [3, 14.5, 11].
```

```python
class Solution:
    def averageOfLevels(self, root: TreeNode) -> List[float]:
        if not root: return 0
        curLevel=[root]
        res=[]
        while len(curLevel)>0:
            nextLevel=[]
            for node in curLevel:
                if node.left:
                    nextLevel.append(node.left)
                if node.right:
                    nextLevel.append(node.right)
            res.append(sum([node.val for node in curLevel])/len(curLevel))
            curLevel=nextLevel
        return res
```



### 863 All Nodes Distance K in Binary Tree

We are given a binary tree (with root node `root`), a `target` node, and an integer value `K`.

Return a list of the values of all nodes that have a distance `K` from the `target` node. The answer can be returned in any order.

![img](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/06/28/sketch0.png)

**Example 1:**

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], target = 5, K = 2

Output: [7,4,1]

Explanation: 
The nodes that are a distance 2 from the target node (with value 5)
have values 7, 4, and 1.



Note that the inputs "root" and "target" are actually TreeNodes.
The descriptions of the inputs above are just serializations of these objects.
```

 

+ Analysis

  + Going down: all nodes in K levels down

  + Going up:

    + distanceK(target.parent, K-1)
    + to top: distanceK(root, K-targetLevel)

  + First traverse once to find the level of target, at the same time memorize the path to target. (DFS)

  + Then, use BFS to find all cascade of distancK lists along the path, on the branches not including the path

  + ```python
    class Solution:
    
        def distanceK(self, root, target, K):
            self.target=target
            self.path=[]
            found = self.DFS(root, target)
            self.path.append(root)
            self.level = len(self.path)-1
            self.pathSet=set(self.path)
            res=[]
            # print([node.val for node in self.path])
            for i in range(len(self.path)):
                res+=self.BFS(self.path[i],K-i)
            return res
    
        def BFS(self, node, level):
            if level==0: return [node.val]
            curLevel=0
            curLevelList=[node]
            while len(curLevelList)>0:
                nextLevelList=[]
                for node in curLevelList:
                    if node.left and node.left not in self.pathSet:
                        nextLevelList.append(node.left)
                    if node.right and node.right not in self.pathSet:
                        nextLevelList.append(node.right)
                curLevelList=nextLevelList
                curLevel += 1
                if curLevel==level:
                    return [node.val for node in curLevelList]
            return []
    
        def DFS(self, node, target):
            if node.val==target.val:
                return True
            return self.checkNode(node.left,target) or self.checkNode(node.right,target)
    
        def checkNode(self, node, target):
            if node:
                if self.DFS(node,target):
                    self.path.append(node)
                    return True
    ```

  + To find the path, can add parent pointer to each node:

    ```python
            def annotate_parent(node, par=None):
                if node:
                    node.par = par
                    annotate_parent(node.left, node)
                    annotate_parent(node.right, node)
    ```

    