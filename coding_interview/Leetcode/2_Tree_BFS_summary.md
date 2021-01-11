### ueue Operations:

```python
from collections import deque
queue=deque([root])
queue.pop() #tail
queue.append([node]) #tail
queue.popleft()  #head
queue.appendleft([node]) #head 
.insert(i,x)
.reverse()
.extend(iterable)
.extendleft(iterable)
```





### Binary Tree Level Order Traversal

+ Recursion

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

+ Queue

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

+ Use simple list rather than queue:

  ```python
  curLevel, ans=[root],[]
  while curLevel in [n for n in curLevel if n]:
      #handle current level
      level= [n.val for n in curLevel]
      nodes.append(level)
      
      #go over all childs of current level
      curLevel=[c for p in curLevel for c in (p.left, p.right)] 
  return ans
  ```

+ Longer but more straightforward code

  ```python
  curLevel=[root]
  while len(curLevel)>0:
      nextLevel=[]
          if node.left:
              nextLevel.append(node.left)
          if node.right:
              nextLevel.append(node.right)
      curLevel=nextLevel
  return level
  ```

  