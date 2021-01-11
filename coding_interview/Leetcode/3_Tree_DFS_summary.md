### Tree DFS
```python
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

### In Order Traversal

+ Recursive:

  ```python
  def inOrder(root):
      if not root: return None
      inOrder(root.left)
      f(root.val)
      inOrder(root.right)
      return 
  ```

+ Iterative:

  ```python
  def inOrder(root):
      stack=[]
      while stack:
          while root:
  			stack.append(root)
              root = root.left
          root=stack.pop()
          f(root.val)
          root=root.right
      return
  ```

  

### Pre-order traversal

+ Recursive

  ```python
  def preOrder(node):
      f(node)
      preOrder(node.left)
      preOrder(node.right)
      return something
  ```

  

+ Iterative

```python
queue = deque([root])
while queue:
    node = queue.popleft()
    f(node)
    if node:
        queue.append(node.left)
        queue.append(node.right)
    return something
```



### DFS

```python
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
            
```
```python
def DFS(cur, visited: set):
    newVisited = visited.copy()
    newVisited.add(cur)
    for nextItem in legalNexts:
        DFS(nextItem, newVisited)
```



### BST

+ Recursive

```python
def traverse(root, node):
    if node.val<root.val: traverse(root.left, node)
    elif node.val>root.val: traverse(root.right, node)
    else: f(root.val)
```

+ Iterative

```python
def traverse(root, node):
    cur = root
    while cur:
        if node.val>cur.val: cur=cur.right
        elif node.val<cur.val: cur=cur.left
        else: f(cur.val)
```



### Mark parent

+ Recursive mark parent:

  ```python
  root.par = None
  def markParent(node):
      if not node: return
      if node.left:
          node.left.par=node
          markParent(node.left)
      if node.right:
          node.right.par=node
          markParent(node.right)
  ```

  

+ Iterative mark parent: (if adding attribute .parent is not allowed, can use a hashmap)

  ```python
  stack=[root]
  root.parent=None
  while stack:
      node=stack.pop()
      if node.left:
          node.left.parent=node
          stack.append.node.left
      if node.right:
          node.right.parent=node
          stack.append.node.right
  ```

  

### prefix sum

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
          #prevent reuse during parallel subtree processing
          h[cur_sum]-=1 
      preorder(root, 0)
      return count
  ```

  

### Check if two trees are equal:

```python
def equals(self, x,y):
    if not x and not y: return True
    if not x or not y: return False
    return x.val==y.val and self.equals(x.left,y.left) and self.equals(x.right, y.right)
```