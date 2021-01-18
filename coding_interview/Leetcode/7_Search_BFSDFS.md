### 690 Employee Importance



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

 

**Note:**

1. One employee has at most one **direct** leader and may have several subordinates.
2. The maximum number of employees won't exceed 2000.

+ analysis: `[id, importance,[child..]]`

  + Assumption: employees are not ordered by id. Fast retrieval of child requires dictionary. 
  + Assumption: no circles in the graph
  + Algorithm:
    + Convert to dict
    + BFS until no child

  ```python
  from collections import deque
  
  class Solution:
      def getImportance(self, employees: List['Employee'], id: int) -> int:
          employeeDict = {item.id: [item.importance, item.subordinates] for item in employees}
          queue = deque([employeeDict[id]])
          res=0
          while queue:
              cur = queue.popleft()
              res+=cur[0]
              for i in cur[1]:
                  queue.append(employeeDict[i])
          return res
  ```

  

### 17. Letter Combinations of a Phone Number

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

+ DFS (backtrack), putting number in dictionary.
+ Assumption: can reuse number, but no duplication in output

```python
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        d = {'2':["a","b","c"], '3':["d","e","f"],'4':["g","h","i"],'5':["j","k","l"], '6':["m","n","o"],'7':["p","q","r","s"],'8':["t","u","v"],'9':["w","x","y","z"]}
        
        res=set()
        if not digits: return res
        def dfs(res,digits):
            if not digits: return res
            if not res: res.add("") # to initialize
            curres=set()
            for word in res:
                for letter in d[digits[0]]:
                    curres.add(word+letter)
            res = curres
            return dfs(res,digits[1:])
        return list(dfs(res,digits))
```



### 46 Permutations

Given an array `nums` of distinct integers, return *all the possible permutations*. You can return the answer in **any order**.

 

**Example 1:**

```
Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

**Example 2:**

```
Input: nums = [0,1]
Output: [[0,1],[1,0]]
```

**Example 3:**

```
Input: nums = [1]
Output: [[1]]
```

+ output = for each start: append output(nums[1:])

```python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        if not nums: return []
        if len(nums)==1: return [nums]
        res = []
        for i in range(len(nums)):
            for right in self.permute(nums[0:i]+nums[i+1:]):
                res.append([nums[i]]+right)
        return res
```



### 22. Generate Parentheses

Medium

Given `n` pairs of parentheses, write a function to *generate all combinations of well-formed parentheses*.

 

**Example 1:**

```
Input: n = 3
Output: ["((()))","(()())","(())()","()(())","()()()"]
```

**Example 2:**

```
Input: n = 1
Output: ["()"]
```

+ Legal paranthesis:
  + at any given point: num left>=num right (l_available<=r_available)
  + in the end: num left==num right
  + dict {string: [l, r]}

```python
class Solution:
    def generateParenthesis(self, n: int) -> List[str]:
        
        def add(cur,l,r):
            if r==0 and l==0: return [cur]
            if l==0: return add(cur+')',l,r-1)
            res=[]
            if l<r:
                res+=add(cur+')',l,r-1)
            res+=add(cur+'(',l-1,r)
            return res                
            
        cur=""
        l,r=n,n
        return add(cur,n,n)
```



### 37. Sudoku Solver

Hard

Write a program to solve a Sudoku puzzle by filling the empty cells.

A sudoku solution must satisfy **all of the following rules**:

1. Each of the digits `1-9` must occur exactly once in each row.
2. Each of the digits `1-9` must occur exactly once in each column.
3. Each of the digits `1-9` must occur exactly once in each of the 9 `3x3` sub-boxes of the grid.

The `'.'` character indicates empty cells.

 

**Example 1:**

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png)

```
Input: board = [["5","3",".",".","7",".",".",".","."],["6",".",".","1","9","5",".",".","."],[".","9","8",".",".",".",".","6","."],["8",".",".",".","6",".",".",".","3"],["4",".",".","8",".","3",".",".","1"],["7",".",".",".","2",".",".",".","6"],[".","6",".",".",".",".","2","8","."],[".",".",".","4","1","9",".",".","5"],[".",".",".",".","8",".",".","7","9"]]
Output: [["5","3","4","6","7","8","9","1","2"],["6","7","2","1","9","5","3","4","8"],["1","9","8","3","4","2","5","6","7"],["8","5","9","7","6","1","4","2","3"],["4","2","6","8","5","3","7","9","1"],["7","1","3","9","2","4","8","5","6"],["9","6","1","5","3","7","2","8","4"],["2","8","7","4","1","9","6","3","5"],["3","4","5","2","8","6","1","7","9"]]
Explanation: The input board is shown above and the only valid solution is shown below:
```

 ```python
class Solution:
    def solveSudoku(self, board: List[List[str]]) -> None:
        def islegal(i,j,num):
            if num in board[i]: return False
            for row in board:
                if num==row[j]: return False
            for row in board[i//3*3:i//3*3+3]:
                if num in row[j//3*3:j//3*3+3]: return False
            return True
    
        def findVacant():
            for i in range(9):
                for j in range(9):
                     if board[i][j]==".":
                            return i,j
            return -1,-1
    
        def canSolve():

            i,j = findVacant()
            if i==-1 and j==-1: return True

            for num in range(1,10):
                if islegal(i,j,str(num)):
                    board[i][j]=str(num)
                    if canSolve(): return True
                    board[i][j]="."
            return False
        
        
        canSolve()
 ```

+ Tip: for backtrack, go to next recursion as soon as find a legal location, not iterating over all legal location



### 79 Word Search

Medium

Given an `m x n` `board` and a `word`, find if the word exists in the grid.

The word can be constructed from letters of sequentially adjacent cells, where "adjacent" cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/11/04/word2.jpg)

```
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
Output: true
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/11/04/word-1.jpg)

```
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "SEE"
Output: true
```

**Example 3:**

![img](https://assets.leetcode.com/uploads/2020/10/15/word3.jpg)

```
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCB"
Output: false
```

```python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        
        def isLegal(i,j):
            if i<0 or j<0 or i>=n or j>=m: return False
            if (i,j) in visited: return False
            return True
        
        def dfs(i,j,word):
            if not word: return True
            if not isLegal(i,j): return False
            if board[i][j]!=word[0]: return False
            visited.add((i,j))
            res = dfs(i+1,j,word[1:]) or dfs(i-1,j,word[1:]) or dfs(i,j+1,word[1:]) or dfs(i,j-1,word[1:])
            visited.remove((i,j))
            return res
        
        
        n = len(board)
        m = len(board[0])
        visited=set()
        for i in range(n):
            for j in range(m):
                if dfs(i,j,word): return True
        return False
```



### 542. 01 Matrix

Medium

Given a matrix consists of 0 and 1, find the distance of the nearest 0 for each cell.

The distance between two adjacent cells is 1.

 

**Example 1:**

```
Input:
[[0,0,0],
 [0,1,0],
 [0,0,0]]

Output:
[[0,0,0],
 [0,1,0],
 [0,0,0]]
```

**Example 2:**

```
Input:
[[0,0,0],
 [0,1,0],
 [1,1,1]]

Output:
[[0,0,0],
 [0,1,0],
 [1,2,1]]
```

+ find nearest: BFS
+ Iterate over each zero with adjacent 1:
  + expand towards numbers, and set number to min(depth, cur) if number is not 0

```python
from collections import deque

class Solution:
    def updateMatrix(self, matrix: List[List[int]]) -> List[List[int]]:
        n,m=len(matrix), len(matrix[0])
        queue=deque((i,j,0) for i in range(n) for j in range(m) if matrix[i][j]==0)
        ans = [[0 for _ in range(m)] for _ in range(n)]
        
        while queue:
            r,c,d = queue.popleft()
            ans[r][c]=d
            for dr, dc in [(0,1),(0,-1),(1,0),(-1,0)]:
                i,j=r+dr, c+dc
                if 0<=i<n and 0<=j<m and matrix[i][j]==1:
                    matrix[i][j]=0
                    queue.append((i,j,d+1))
        return ans
```

