

### 78. Subsets

Medium

5082106Add to ListShare

Given an integer array `nums`, return *all possible subsets (the power set)*.

The solution set must not contain duplicate subsets.

 

**Example 1:**

```
Input: nums = [1,2,3]
Output: [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```

**Example 2:**

```
Input: nums = [0]
Output: [[],[0]]
```

+ Combination problem

+ recursion: 

  + ```python
    class Solution:
        def subsets(self, nums: List[int]) -> List[List[int]]:
            def subsetHelper(nums, curIdx, prePath, res):
                if curIdx == len(nums):
                    res.append(prePath)
                    return
                subsetHelper(nums, curIdx+1, prePath, res)
                subsetHelper(nums, curIdx+1, prePath+[nums[curIdx]],res)
                return
            
            path = []
            results = []
            subsetHelper(nums,0,path,results)
            return results
            
    ```



### 131. Palindrome Partitioning

Medium

286393Add to ListShare

Given a string `s`, partition `s` such that every substring of the partition is a **palindrome**. Return all possible palindrome partitioning of `s`.

A **palindrome** string is a string that reads the same backward as forward.

 

**Example 1:**

```
Input: s = "aab"
Output: [["a","a","b"],["aa","b"]]
```

**Example 2:**

```
Input: s = "a"
Output: [["a"]]
```

+ s.length<=16, not very long, can do recursion

+ for end in 0, length-1, check if start-end is palidrome, if yes, recurse on remaining

+ base case:

+ no char left: true

+ Added memo, 2x faster (need DP to get even faster)

+ ```python
      def partitionHelper(start, path, res):
          if start>=len(s): 
              res.append(path)
              return
  
          for end in range(start, len(s)):
              if isPalindrome(start, end):
                  partitionHelper(end+1, path+[s[start:end+1]], res)
          return
  
      def isPalindrome(start, end):
          if start==end: return True
          if (start,end) in memo:
              return memo[(start,end)]
          l, r = start, end
          while l<r:
              if s[l]==s[r]:
                  l+=1
                  r-=1
              else:
                  memo[(start,end)]=False
                  return False
          memo[(start,end)]=True
          return True
  
  
      memo=defaultdict()
  
      curPath=[]
      res=[]
      partitionHelper(0,curPath,res)
      return res
  ```

+ 

### 17. Letter Combinations of a Phone Number

Medium

Given a string containing digits from `2-9` inclusive, return all possible letter combinations that the number could represent. Return the answer in **any order**.

A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/7/73/Telephone-keypad2.svg/200px-Telephone-keypad2.svg.png)

 

**Example 1:**

```
Input: digits = "23"
Output: ["ad","ae","af","bd","be","bf","cd","ce","cf"]
```

**Example 2:**

```
Input: digits = ""
Output: []
```

**Example 3:**

```
Input: digits = "2"
Output: ["a","b","c"]
```



```python
    if not digits: return []


    def combination(idx, prevPath, res):
        if idx==len(digits):
            res.append(prevPath)
            return

        for c in d[digits[idx]]:
            combination(idx+1, prevPath+c, res)
        return


    d = {"2":["a","b","c"], "3":["d","e","f"], "4":["g","h","i"], "5":["j","k","l"], "6":["m","n","o"], "7":["p","q","r","s"], "8":["t","u","v"], "9":["w","x","y","z"]}

    path = ""
    res=[]
    combination(0, path, res)
    return res
```



### 490. The Maze

Medium

There is a ball in a `maze` with empty spaces (represented as `0`) and walls (represented as `1`). The ball can go through the empty spaces by rolling **up, down, left or right**, but it won't stop rolling until hitting a wall. When the ball stops, it could choose the next direction.

Given the `maze`, the ball's `start` position and the `destination`, where `start = [startrow, startcol]` and `destination = [destinationrow, destinationcol]`, return `true` if the ball can stop at the destination, otherwise return `false`.

You may assume that **the borders of the maze are all walls** (see examples).

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/09/01/maze1.png)

```
Input: maze = [[0,0,1,0,0],[0,0,0,0,0],[0,0,0,1,0],[1,1,0,1,1],[0,0,0,0,0]], start = [0,4], destination = [4,4]
Output: true
Explanation: One possible way is : left -> down -> left -> down -> right -> down -> right.
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/09/01/maze2.png)

```
Input: maze = [[0,0,1,0,0],[0,0,0,0,0],[0,0,0,1,0],[1,1,0,1,1],[0,0,0,0,0]], start = [0,4], destination = [3,2]
Output: false
Explanation: There is no way for the ball to stop at the destination. Notice that you can pass through the destination but you cannot stop there.
```

**Example 3:**

```
Input: maze = [[0,0,0,0,0],[1,1,0,0,1],[0,0,0,0,0],[0,1,0,0,1],[0,1,0,0,0]], start = [4,3], destination = [0,1]
Output: false
```

```python
class Solution:
    def hasPath(self, maze: List[List[int]], start: List[int], destination: List[int]) -> bool:
        
        def hasPathHelper(maze,start,destination,visited):
            if start==destination: return True
            # 0: legal, 1: illegal
            if not maze: return True
            i,j = start
            if maze[i][j]==1 or (i,j) in visited: return False
            visited.add((i,j))
            for nextIdx in roll(start):
                if hasPathHelper(maze, nextIdx, destination, visited):
                    return True
            return False
        
        def roll(start):
            res=set()
            i, j = start
            for row in range(i+1, n+1):
                if row==n or maze[row][j]==1:
                    res.add((row-1,j))
                    break
            for col in range(j+1, m+1):
                if col==m or maze[i][col]==1:
                    res.add((i,col-1))
                    break
            for row in range(i-1, -2,-1):
                if row==-1 or maze[row][j]==1:
                    res.add((row+1,j))
                    break
            for col in range(j-1, -2,-1):
                if col==-1 or maze[i][col]==1:
                    res.add((i,col+1))
                    break
            if start in res:
                res.remove(start)
            return list(res)

        n,m=len(maze), len(maze[0])
        visited=set()
        return hasPathHelper(maze,tuple(start),tuple(destination),visited)
```



### 51. N-Queens

Hard

The **n-queens** puzzle is the problem of placing `n` queens on an `n x n` chessboard such that no two queens attack each other.

Given an integer `n`, return *all distinct solutions to the **n-queens puzzle***.

Each solution contains a distinct board configuration of the n-queens' placement, where `'Q'` and `'.'` both indicate a queen and an empty space, respectively.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/11/13/queens.jpg)

```
Input: n = 4
Output: [[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]
Explanation: There exist two distinct solutions to the 4-queens puzzle as shown above
```

**Example 2:**

```
Input: n = 1
Output: [["Q"]]
```

 

**Constraints:**

- `1 <= n <= 9`



```python
class Solution:
    def solveNQueens(self, n: int) -> List[List[str]]:
        
        def dfs(row,col, path, res):
            if not isLegal(row, col, path): return
            if row==n-1: 
                savePath(path ,res)
                return
            for nextCol in range(n):
                dfs(row+1, nextCol, path+[nextCol],res)
            return
        
        def savePath(path, res):
            cur = []
            for loc in path:
                cur.append('.'*loc+'Q'+'.'*(n-loc-1))
            res.append(cur)
            return
        
        def isLegal(row, col, path):
            for i, j in enumerate(path):
                if i==row and j==col: break
                if j==col: return False
                if col-j == row-i or col-j== i-row: return False
            return True
            
                
            
        path=[]
        res=[]
        for col in range(n):
            dfs(0, col, path+[col], res)
        return res
```



### 212 Word Search

### 401 Binary Watch



Easy

A binary watch has 4 LEDs on the top which represent the **hours** (**0-11**), and the 6 LEDs on the bottom represent the **minutes** (**0-59**).

Each LED represents a zero or one, with the least significant bit on the right.

![img](https://upload.wikimedia.org/wikipedia/commons/8/8b/Binary_clock_samui_moon.jpg)

For example, the above binary watch reads "3:25".

Given a non-negative integer *n* which represents the number of LEDs that are currently on, return all possible times the watch could represent.

**Example:**

```
Input: n = 1
Return: ["1:00", "2:00", "4:00", "8:00", "0:01", "0:02", "0:04", "0:08", "0:16", "0:32"]
```

+ 10 slots in total, different combination of the slots must give different time
+ take n from 10 problem, then convert to time

```python
class Solution:
    def readBinaryWatch(self, num: int) -> List[str]:
        def combination(idx, prevPath, res):
            if len(prevPath)==num:
                savePath(prevPath.copy(),res)
                return
            if idx>=10:
                return
            combination(idx+1, prevPath+[idx], res)
            combination(idx+1, prevPath,res)
            return
        
        def savePath(path, res):
            hour, minutes = 0,0
            for val in path:
                if val>=6:
                    hour+=1<<(val-6)
                else:
                    minutes+= 1<<val
            if hour>11:
                return
            res.append("%d:%02d" % (hour, minutes) )
            return
                    
        
        path=[]
        res=[]
        combination(0, path, res)
        return res
        
```



```python
### Sorted in time:

return [
	"%d:%02d" % (h, m) 
	for h in range(12) for m in range(60)
	if (bin(h) + bin(m)).count("1") == num
]
```



### 40. Combination Sum II

Medium

Given a collection of candidate numbers (`candidates`) and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sum to `target`.

Each number in `candidates` may only be used **once** in the combination.

**Note:** The solution set must not contain duplicate combinations.

 

**Example 1:**

```
Input: candidates = [10,1,2,7,6,1,5], target = 8
Output: 
[
[1,1,6],
[1,2,5],
[1,7],
[2,6]
]
```

**Example 2:**

```
Input: candidates = [2,5,2,1,2], target = 5
Output: 
[
[1,2,2],
[5]
]
```

```python
class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        def combination(idx, target, prevPath, res):
            if target==0:
                res.append(tuple(prevPath))
                return
            if target<0 or idx>=51:
                return
            for count in range(counts[idx]+1):
                combination(idx+1, target-count*idx,prevPath+[idx]*count, res)
            return
        
        counts=[0]*51
        for item in candidates:
            counts[item]+=1
        path=[]
        res=[]
        combination(0, target, path, res)
        return res
```

+ Sorting is required to prevent duplication for list with duplications

```python
def combination(idx, target, path, res):
    if target==0:
        res.append(path.copy())
        return
    for nextIdx in range(idx, len(candidates)):
        if nextIdx>idx and candidates[nextIdx]==candidates[nextIdx-1]:
            # if repeating item, skip all repetetions (they will be accounted in recursion from idx+1)
            continue

        cur = candidates[nextIdx] # starting point
        if target-cur<0: break # skip remaining recursions
        path.append(cur)
        combination(nextIdx+1, target-cur, path, res)
        path.pop()

candidates.sort() # sort to prevent duplication
path, res = [], []
combination(0, target, path, res)


return res
```



### subset sums

+ nums: distinct, not ordered
+ output: list of list, ordered, subsets of nums (can be not contiguous)

def  subsetSums(nums, target):
       
```python
def helper(target, index, path, res):
    if index==len(nums):
        return
    cur = nums[index]
    if target==cur:
        res.append(path.copy()+[cur])
        return
    if target>cur:
        path.append(cur)
        helper(target-cur, index+1, path, res)
        path.pop()
    helper(target, index+1, path, res)
    return

nums.sort()

if not nums: return []
path, res = [], []
helper(target, 0, path, res)
return res    
```


### Form polygon

+ requirement: use up all inputs, make one polygon with equal sides
+ partition the input into equal sum subsets (number of subsets>2)
+ Input: list of lengths (len<100)
+ output: true/ false



+ Analysis

  + Brute force: 
    + find sum
    + start from k=3, sum/k,  to find partitions
    + for each partition, only need to find k-1 subsets with sum of sum/k

+ ```python
  def  formPolygon(sticks, sides):
      
      
      def helper(target, index,tovisit):
          
          if index==len(nums):
              return False
          if index not in tovisit:
              return helper(target, index+1, tovisit)
          
          cur = nums[index]
          
          if target==cur:
              tovisit.remove(index)
              return True
          if target>cur:
              if helper(target-cur, index+1, tovisit):
                  tovisit.remove(index)
                  return True
          return helper(target, index+1, tovisit)
      
      
      if not sticks or len(sticks)<sides: return False
      if sum(sticks) % sides != 0: return False
      sticks.sort(reverse = True)
      target = sum(sticks)//sides
      tovisit = set([i for i in range(len(sticks))])
      for i in range(sides-1):
          res = helper(target, 0, tovisit)
          if not res: return False
      
      remaining = 0
      for i in tovisit:
          remaining+=sticks[i]
      return remaining==target
      
      
  ```



### Lucky Numbers

888 is a lucky number. And for each American phone number, we can actually add some operators to make it become 888. For example:phone number is 7765332111, you will have

`7/7*65*3+3*21*11=888`

`776+5+3*32+11*1 = 888`

We want to get a full list of all the operation equations that can get a certain lucky number

Additional information: String num will always be 10 digits since it is a phone number. we can have "0" but we cannot have "05", "032".If a number cannot be divided, you cannot use it. For example, 55/2 is not allowed, you need to make sure the division can always be an integer result. cannot be divided.

+ input: str(nums), target
+ output: list of strings (nums and operators)



+ Analaysis
  + Brute: We are outputting all of them, we have to go through $O(4^n)$ trials. Insert each operator, check whether expression is legal, then evaluate and see if it is found
  +  Recursion: 
    + if prev path is >= target, return
    + then try different combinations

Max time limit exceeded

```python
def luckyNumbers(num, target):
    
    def helper(index, path, res):
        if index>=len(num): return
        candidates = []
        
        prevnum=''
        i = len(path)-1
        while i>=0 and path[i].isdigit():
            prevnum = path[i]+prevnum
            i-=1
        if i<=0 or path[i]!='/':
            candidates.append(path+num[index])
        if i>0 and path[i]=='/':
            curnum = prevnum
            prevnum=''
            i-=1
            while i>=0 and path[i].isdigit():
                prevnum = path[i]+prevnum
                i-=1
            if eval(prevnum+'.0'+'/'+curnum+num[index]+'.0')%1==0.0:
                print("added")
                candidates.append(path+num[index])
  

        if num[index]!='0':
            candidates.append(path+'+'+num[index])
            candidates.append(path+'-'+num[index])
            candidates.append(path+'*'+num[index])
            if eval(prevnum+'.0'+'/'+num[index]+'.0')%1==0:
                candidates.append(path+'/'+num[index])
                
                
        for candidate in candidates:
            val = eval(candidate)
            if val==target and index==len(num)-1:
                res.append(candidate)
            else:
                helper(index+1, candidate, res)
        return
    
    if not num: return []
    if len(num)==1: return int(num)==target
    path=num[0]
    res=[]
    helper(1,path,res)
    
    return res
    
    
    
tests = [["7765332111",888]]
for i,j in tests:
    print(luckyNumbers(i,j))
```



### 93. Restore IP Addresses

Medium

Given a string `s` containing only digits, return all possible valid IP addresses that can be obtained from `s`. You can return them in **any** order.

A **valid IP address** consists of exactly four integers, each integer is between `0` and `255`, separated by single dots and cannot have leading zeros. For example, "0.1.2.201" and "192.168.1.1" are **valid** IP addresses and "0.011.255.245", "192.168.1.312" and "192.168@1.1" are **invalid** IP addresses. 

 

**Example 1:**

```
Input: s = "25525511135"
Output: ["255.255.11.135","255.255.111.35"]
```

**Example 2:**

```
Input: s = "0000"
Output: ["0.0.0.0"]
```

**Example 3:**

```
Input: s = "1111"
Output: ["1.1.1.1"]
```

**Example 4:**

```
Input: s = "010010"
Output: ["0.10.0.10","0.100.1.0"]
```

**Example 5:**

```
Input: s = "101023"
Output: ["1.0.10.23","1.0.102.3","10.1.0.23","10.10.2.3","101.0.2.3"]
```

+ problem: partition numbers into four parts
+ constraints:
  + integers can be zero, but cannot start with zero otherwise
  + integers between 0 and 255

```python
class Solution:
    def restoreIpAddresses(self, s: str) -> List[str]:
        
        
        def helper(index, path, res):         
            if len(path)>4:
                return
            if index>=len(s): 
                if len(path)==4:
                    res.append('.'.join(path))
                return

            for i in range(1,4):
                if index+i>len(s):
                    continue
                cur = s[index:index+i]

                if int(cur)>255:
                    continue
                path.append(cur)
                helper(index+i,path,res)
                path.pop()
                if cur=='0':
                    break
                
            return
        
        path=[]
        res=[]
        helper(0, path, res)
        return res
```



### 690. Employee Importance

Easy

You are given a data structure of employee information, which includes the employee's **unique id**, their **importance value** and their **direct** subordinates' id.



For example, employee 1 is the leader of employee 2, and employee 2 is the leader of employee 3. They have importance value 15, 10 and 5, respectively. Then employee 1 has a data structure like [1, 15, [2]], and employee 2 has [2, 10, [3]], and employee 3 has [3, 5, []]. Note that although employee 3 is also a subordinate of employee 1, the relationship is **not direct**.

Now given the employee information of a company, and an employee id, you need to return the total importance value of this employee and all their subordinates.

**Example 1:**

```
Input: [[1, 5, [2, 3]], [2, 3, []], [3, 3, []]], 1
Output: 11
Explanation:
Employee 1 has importance value 5, and he has two direct subordinates: employee 2 and employee 3. They both have importance value 3. So the total importance value of employee 1 is 5 + 3 + 3 = 11.
```



+ Input: `[id, imortance, [direct_child...]], Int(id)`
+ output: `sum(values from cur Node to the leaf)=> Int`
+ Assumptions:
  + No circles in the graph: if there is a circle, have inf importance value
  + imp_values>0
  + each node has at most one direct parent (this may not matter)
  + each node has >=0 children (can be larger than 2, not necessarily binary tree)
  + Not necessarily ordered
  + all children and given id are within the list
+ Algorithm:
  + employeeMap = {id: [imp_val, [children]]}
  + queue
  + start from the root (given index, i), queue.append(root)
  + queue.add(children of the root)
  + while queue not empty: queue.pop() queue.append(children)



### 695. Max Area of Island

Medium

Given a non-empty 2D array `grid` of 0's and 1's, an **island** is a group of `1`'s (representing land) connected 4-directionally (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

Find the maximum area of an island in the given 2D array. (If there is no island, the maximum area is 0.)

**Example 1:**

```
[[0,0,1,0,0,0,0,1,0,0,0,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,1,1,0,1,0,0,0,0,0,0,0,0],
 [0,1,0,0,1,1,0,0,1,0,1,0,0],
 [0,1,0,0,1,1,0,0,1,1,1,0,0],
 [0,0,0,0,0,0,0,0,0,0,1,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,0,0,0,0,0,0,1,1,0,0,0,0]]
```

Given the above grid, return `6`. Note the answer is not 11, because the island must be connected 4-directionally.

**Example 2:**

```
[[0,0,0,0,0,0,0,0]]
```

Given the above grid, return `0`.

**Note:** The length of each dimension in the given `grid` does not exceed 50.



+ input: 2D array 0 1's 

+ island: 

  + group of 1's (UDLR)
  + surrounded by 0's or edge

+ Output:  Int(max area)

  + over all islands

+ Assumptions:

  + `>=0` islands, if `=0`, return 0
  + no grid: return 0
  + a single one: return 1

+ Algorithm

  ```
  for curIdx in arrayIdxes:
  	if array[curIdx]==1:
  		size = 1
  		bfs(curIdx, curSize)
  
  bfs: UDLR if legal(nexIdx)
  
  legal(Idx)
  ```

+ ```python
  def bfs(i,j):
      ni = [1,0,-1,0]
      nj = [0,1,0,-1]
      curSize = 0
      queue = deque([(i,j)])
      grid[i][j]=0
  
      while queue:
          i,j = queue.popleft()
          curSize += 1
          for k in range(4):
              ci = i+ni[k]
              cj = j+nj[k]
              if 0<=ci<n and 0<=cj<m and grid[ci][cj]==1:
                  grid[ci][cj]=0
                  queue.append((ci,cj))
      return curSize
  
  
  
  if not grid: return 0
  maxSize = 0
  n = len(grid)
  m= len(grid[0])
  for i in range(n):
      for j in range(m):
          if grid[i][j]==1:
              curSize = bfs(i,j)
              maxSize = max(curSize, maxSize)
  return maxSize
  ```



### 364. Nested List Weight Sum II

Medium

Given a nested list of integers, return the sum of all integers in the list weighted by their depth.

Each element is either an integer, or a list -- whose elements may also be integers or other lists.

Different from the [previous question](https://leetcode.com/problems/nested-list-weight-sum/) where weight is increasing from root to leaf, now the weight is defined from bottom up. i.e., the leaf level integers have weight 1, and the root level integers have the largest weight.

**Example 1:**

```
Input: [[1,1],2,[1,1]]
Output: 8 
Explanation: Four 1's at depth 1, one 2 at depth 2.
```

**Example 2:**

```
Input: [1,[4,[6]]]
Output: 17 
Explanation: One 1 at depth 3, one 4 at depth 2, and one 6 at depth 1; 1*3 + 4*2 + 6*1 = 17.
```

+ Input: List[Object: NestedInteger]
  + weight: height of tree
+ Output: sum of all integers weighted by depth (sum(int*depth))
+ Analysis
  + `[1,[4,[6]]]`
  + num_layers = 3
  + layers: sum = 3+8+6 = 17
    + 1: 1*3
    + 2: 4*2
    + 3: 6*1
+ Data structure
  + For each item:
    + isInteger:
      + True: getInteger
      + False: getList
+ Algorithm:
  + BFS w/ memorizing the level number
  + `[1,[4,[6]]]`0
    + 1, `[4,[6]]`; 1
      + `[4,[6]]`
    + `[4,[6]]`; 2
    + `[6]`3

```python
# """
# This is the interface that allows for creating nested lists.
# You should not implement it, or speculate about its implementation
# """
#class NestedInteger:
#    def __init__(self, value=None):
#        """
#        If value is not specified, initializes an empty list.
#        Otherwise initializes a single integer equal to value.
#        """
#
#    def isInteger(self):
#        """
#        @return True if this NestedInteger holds a single integer, rather than a nested list.
#        :rtype bool
#        """
#
#    def add(self, elem):
#        """
#        Set this NestedInteger to hold a nested list and adds a nested integer elem to it.
#        :rtype void
#        """
#
#    def setInteger(self, value):
#        """
#        Set this NestedInteger to hold a single integer equal to value.
#        :rtype void
#        """
#
#    def getInteger(self):
#        """
#        @return the single integer that this NestedInteger holds, if it holds a single integer
#        Return None if this NestedInteger holds a nested list
#        :rtype int
#        """
#
#    def getList(self):
#        """
#        @return the nested list that this NestedInteger holds, if it holds a nested list
#        Return None if this NestedInteger holds a single integer
#        :rtype List[NestedInteger]
#        """

from collections import deque

class Solution:
    def depthSumInverse(self, nestedList: List[NestedInteger]) -> int:
        if not nestedList: return 0

        queue = deque(nestedList)
        level = 0
        while queue:
            queue2 = deque()
            while queue:
                cur = queue.popleft()
                if not cur.isInteger():
                    for item in cur.getList():
                        queue2.append(item)
            level+=1
            queue=queue2


        # level == height

        res = 0
        queue = deque(nestedList)
        while queue:
            queue2 = deque()
            while queue:
                cur = queue.popleft()

                if cur.isInteger():
                    res += cur.getInteger()*level
                else:
                    for item in cur.getList():
                        queue2.append(item)
            level-=1
            queue=queue2

        return res
        
        
        
        
```



### 494. Target Sum

Medium

You are given a list of non-negative integers, a1, a2, ..., an, and a target, S. Now you have 2 symbols `+` and `-`. For each integer, you should choose one from `+` and `-` as its new symbol.

Find out how many ways to assign symbols to make sum of integers equal to target S.

**Example 1:**

```
Input: nums is [1, 1, 1, 1, 1], S is 3. 
Output: 5
Explanation: 

-1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3

There are 5 ways to assign symbols to make the sum of nums be target 3.
```

+ Input: List[Int], taget

+ Output: Int

  + num ways to achieve the target

+ Assumption: 

  + Int>=0 , S>=0
  + two symbols: +, -
  + in front of each int
  + are duplication in list
  + no duplication in output
  + len(arr)<=20
  + Sum<=1000

+ Brute force recursion, TLE:

  ```python
  def helper(index, curSum):
      if index>=len(nums):
          if curSum == 0:
              totSols[0] += 1
          return
  
      helper(index+1, curSum-nums[index]) # "+"
      helper(index+1, curSum+nums[index]) # "-"
      return
  
  totSols = [0]
  helper(0, S)
  return totSols[0]
  ```

  

+ DP: `[1,1,1,1,1], 3`
  + -> Sum
  + v: index
  + content: num of ways to achieve cur sum with numbers up to that index 

|       | -5   | -4   | -3   | -2   | -1   | 0    | 1    | 2    | 3    | 4    | 5    |
| ----- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| 0 (1) |      |      |      |      | 1    |      | 1    |      |      |      |      |
| 1 (1) |      |      |      | 1    |      | 2    |      | 1    |      |      |      |
| 2 (1) |      |      | 1    |      | 3    |      | 3    |      | 1    |      |      |
| 3 (1) |      | 1    |      | 4    |      | 6    |      | 4    |      | 1    |      |
| 4 (1) | 1    |      | 5    |      | 10   |      | 10   |      | 5    |      | 1    |

   + Symmetric positive and negative

   + Each `dp[i][j]=dp[i-1][j-nums[i]]+dp[i-1][j+nums[i]] `

   + `max(j)<=sum(nums)`

   + ```python
     class Solution:
         def findTargetSumWays(self, nums: List[int], S: int) -> int:
             if not nums: return 0
             tot = sum(nums)
             if tot<S:
                 return 0
             dp = [0]*(2*tot+1)
             dp[tot] = 1
             for num in nums:
                 prev = dp.copy()
                 for i in range(0, len(dp)):
                     left = prev[i-num] if i-num>=0 else 0
                     right = prev[i+num] if i+num<len(dp) else 0
                     dp[i] = left + right
                     
             return dp[tot+S]
     ```

   + 

   + Save one side:

   + ```python
     class Solution:
         def findTargetSumWays(self, nums: List[int], S: int) -> int:
             if not nums: return 0
             tot = sum(nums)
             if tot<S:
                 return 0
             dp = [0]*(tot+1)
             dp[0] = 1
             for num in nums:
                 prev = dp.copy()
                 for i in range(0, len(dp)):
                     left = prev[i-num] if i-num>=0 else prev[num-i]
                     right = prev[i+num] if i+num<len(dp) else 0
                     dp[i] = left + right 
             return dp[abs(S)]
     ```

```java
class Solution {
    public int findTargetSumWays(int[] nums, int S) {
        if (nums.length == 0)
            return 0;
        int sum = Arrays.stream(nums).sum();
        if (sum < S)
            return 0;
        int len = sum + 1;
        int[] dp = new int[len];
        dp[0] = 1;
        for (int num: nums){
            int[] prev = dp.clone();
            for (int i=0; i<len; i++){
                int left = (i-num >= 0) ? prev[i - num] : prev[num - i];
                int right = (i+num < len) ? prev[i + num] : 0;
                dp[i] = left + right;
            }
        }
        
        return dp[Math.abs(S)];
        
    }
```



### 127. Word Ladder

Hard

A **transformation sequence** from word `beginWord` to word `endWord` using a dictionary `wordList` is a sequence of words such that:

- The first word in the sequence is `beginWord`.
- The last word in the sequence is `endWord`.
- Only one letter is different between each adjacent pair of words in the sequence.
- Every word in the sequence is in `wordList`.

Given two words, `beginWord` and `endWord`, and a dictionary `wordList`, return *the **number of words** in the **shortest transformation sequence** from* `beginWord` *to* `endWord`*, or* `0` *if no such sequence exists.*

 

**Example 1:**

```
Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
Output: 5
Explanation: One shortest transformation is "hit" -> "hot" -> "dot" -> "dog" -> "cog" with 5 words.
```

**Example 2:**

```
Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log"]
Output: 0
Explanation: The endWord "cog" is not in wordList, therefore there is no possible transformation.
```

 

**Constraints:**

- `1 <= beginWord.length <= 10`
- `endWord.length == beginWord.length`
- `1 <= wordList.length <= 5000`
- `wordList[i].length == beginWord.length`
- `beginWord`, `endWord`, and `wordList[i]` consist of lowercase English letters.
- `beginWord != endWord`
- All the strings in `wordList` are **unique**.





+ Use intermediate word to represent the word's adjacent word

```python
from collections import defaultdict
class Solution(object):
    def ladderLength(self, beginWord, endWord, wordList):
        """
        :type beginWord: str
        :type endWord: str
        :type wordList: List[str]
        :rtype: int
        """

        if endWord not in wordList or not endWord or not beginWord or not wordList:
            return 0

        # Since all words are of same length.
        L = len(beginWord)

        # Dictionary to hold combination of words that can be formed,
        # from any given word. By changing one letter at a time.
        all_combo_dict = defaultdict(list)
        for word in wordList:
            for i in range(L):
                # Key is the generic word
                # Value is a list of words which have the same intermediate generic word.
                all_combo_dict[word[:i] + "*" + word[i+1:]].append(word)


        # Queue for BFS
        queue = collections.deque([(beginWord, 1)])
        # Visited to make sure we don't repeat processing same word.
        visited = {beginWord: True}
        while queue:
            current_word, level = queue.popleft()
            for i in range(L):
                # Intermediate words for current word
                intermediate_word = current_word[:i] + "*" + current_word[i+1:]

                # Next states are all the words which share the same intermediate state.
                for word in all_combo_dict[intermediate_word]:
                    # If at any point if we find what we are looking for
                    # i.e. the end word - we can return with the answer.
                    if word == endWord:
                        return level + 1
                    # Otherwise, add it to the BFS Queue. Also mark it visited
                    if word not in visited:
                        visited[word] = True
                        queue.append((word, level + 1))
                all_combo_dict[intermediate_word] = []
        return 0
```



### Palindrome Partitioning 

### Sudoku Solver



### 