

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



### Palindrome Partitioning Restore IP Address
### Sudoku Solver