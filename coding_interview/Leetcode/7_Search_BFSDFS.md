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



### OA3 number of different size islands

Given a 2d grid map of '1's (land) and '0's (water), count the number of islands with different sizes. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.





输入描述

a two-d array of integer.  

输出描述

number of islands with different sizes



样例输入

```
5
5
1 1 0 0 0
1 1 0 0 0
0 0 0 0 0
0 0 0 0 1
0 0 1 1 1
```

样例输出

```
1
```

+ Iterate over the matrix to find 1
+ at a 1: start traversing (DFS or BFS) (count = 0)
  + see 0 or out of bound: illegal
  + see one: turn 1 into 0 (count+=1)
  + Once no more 1, exit the search

```python
    def search(i,j, grids, count):
        di = [1, 0, -1, 0]
        dj = [0, 1, 0, -1]
        for k in range(4):
            newi, newj = i+di[k], j+dj[k]
            if 0<=newi<rows and 0<=newj<cols and grids[newi][newj]==1:
                grids[newi][newj]=0
                count[0]+=1
                search(newi, newj, grids, count)
        return
    
    
    
    if not grids: return 0
    rows, cols = len(grids), len(grids[0])
    counts = set()
    for i in range(rows):
        for j in range(cols):
            if grids[i][j]==1:
                count = [0]
                search(i,j, grids, count)
                counts.add(count[0])
    return len(counts)
```



### 472. Concatenated Words

Hard

Given a list of words (**without duplicates**), please write a program that returns all concatenated words in the given list of words.

A concatenated word is defined as a string that is comprised entirely of at least two shorter words in the given array.

 

**Example 1:**

```
Input: words = ["cat","cats","catsdogcats","dog","dogcatsdog","hippopotamuses","rat","ratcatdogcat"]
Output: ["catsdogcats","dogcatsdog","ratcatdogcat"]
Explanation: "catsdogcats" can be concatenated by "cats", "dog" and "cats"; 
"dogcatsdog" can be concatenated by "dog", "cats" and "dog"; 
"ratcatdogcat" can be concatenated by "rat", "cat", "dog" and "cat".
```

**Example 2:**

```

Input: words = ["cat","dog","catdog"]
Output: ["catdog"]
```



+ input: List(String):
  + some are single words, some are concatenated words, need to find the concatenated words
+ Output: all concatenated words
+ Assumptions:
  + input: no duplicate
  + words.length<10^4
  + words[i].length<1000
  + sum(words[i].length)<6E5

+ catndog:

  + c: F
  + ca: F
  + cat: Y 
    + n: Y
    + dog: Y

+ datastructures:

  + set: to find if the word is in the set
  + order words by length

  

```python
def findAllConcatenated(words):
    def isConcat(word):
        if word in memo: return memo[word]
        if len(word)<=1: return False
        for nextStart in range(1,len(word)):
            if word[0:nextStart] in wordsSet:
                nextWord = word[nextStart:]
                if nextWord in wordsSet or isConcat(nextWord):
                    memo[word]=True
                    return True
        memo[word]=False
        return False

    words.sort(key=len)
    wordsSet = set(words)
    memo = defaultdict()
    res = []
    for word in words:
        if isConcat(word):
            res.append(word)
    return res
```



### 366. Find Leaves of Binary Tree

Medium

Given a binary tree, collect a tree's nodes as if you were doing this: Collect and remove all leaves, repeat until the tree is empty.

 

**Example:**

```
Input: [1,2,3,4,5]
  
          1
         / \
        2   3
       / \     
      4   5    

Output: [[4,5,3],[2],[1]]
```

 

**Explanation:**

\1. Removing the leaves `[4,5,3]` would result in this tree:

```
          1
         / 
        2          
```

 

\2. Now removing the leaf `[2]` would result in this tree:

```
          1          
```

 

\3. Now removing the leaf `[1]` would result in the empty tree:

```
          []         
```

[[3,5,4],[2],[1]], [[3,4,5],[2],[1]], etc, are also consider correct answers since per each level it doesn't matter the order on which elements are returned.

+ Go other the tree, once the child is leaf, turn that into None. Finish with all leaves, and traverse again.

```python
    def removeLeaves(node, curRes):
        if not node: return
        if not node.left and not node.right:
            curRes.append(node.val)
            return
        if node.left:
            if not node.left.left and not node.left.right:
                curRes.append(node.left.val)
                node.left = None
            else:
                removeLeaves(node.left , curRes)
        if node.right:
            if not node.right.left and not node.right.right:
                curRes.append(node.right.val)
                node.right = None
            else:
                removeLeaves(node.right , curRes)
        return


    res = []
    dummy = TreeNode(0)
    dummy.left  = root
    while dummy.left:
        curRes = []
        removeLeaves(dummy, curRes)
        res.append(curRes)
    return res
```



### 559. Maximum Depth of N-ary Tree

Easy

Given a n-ary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

*Nary-Tree input serialization is represented in their level order traversal, each group of children is separated by the null value (See examples).*

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2018/10/12/narytreeexample.png)

```
Input: root = [1,null,3,2,4,null,5,6]
Output: 3
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2019/11/08/sample_4_964.png)

```
Input: root = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]
Output: 5
```

```python
class Solution:
    def maxDepth(self, root: 'Node') -> int:
        if not root: return 0
        maxDepth = 1
        for child in root.children:
            maxDepth = max(maxDepth, self.maxDepth(child)+1)
        return maxDepth
```



### 101 Symmetric Tree

```python
class Solution:
    def isSymmetric(self, root: TreeNode) -> bool:
        def symmetric(node1, node2):
            if not node1 and not node2: return True
            if node1 and node2:
                if node1.val!=node2.val: return False
                return symmetric(node1.left, node2.right) and symmetric(node1.right, node2.left)
            return False
        
        if not root: return True
        return symmetric(root.left, root.right)
```



### 666. Path Sum IV

Medium

If the depth of a tree is smaller than `5`, then this tree can be represented by a list of three-digits integers.

For each integer in this list:

1. The hundreds digit represents the depth `D` of this node, `1 <= D <= 4.`
2. The tens digit represents the position `P` of this node in the level it belongs to, `1 <= P <= 8`. The position is the same as that in a full binary tree.
3. The units digit represents the value `V` of this node, `0 <= V <= 9.`

Given a list of `ascending` three-digits integers representing a binary tree with the depth smaller than 5, you need to return the sum of all paths from the root towards the leaves.

It's guaranteed that the given list represents a valid connected binary tree.

**Example 1:**

```
Input: [113, 215, 221]
Output: 12
Explanation: 
The tree that the list represents is:
    3
   / \
  5   1

The path sum is (3 + 5) + (3 + 1) = 12.
```

 

**Example 2:**

```
Input: [113, 221]
Output: 4
Explanation: 
The tree that the list represents is: 
    3
     \
      1

The path sum is (3 + 1) = 4.
```

+ Input: ascending: then the input is ordered, we just populate the tree with BFS directly
+ no need to construct tree, find by index directly. Construct a dict of {level+loc: number}

```python
def find(loc, child):
    level, idx = loc//10, loc%10
    if child=="left":
        targetloc = (level+1)*10 + 2*idx-1
    else:
        targetloc = (level+1)*10 + 2*idx
    val = sys.maxsize
    if targetloc in treeDict:
        val = treeDict[targetloc]
    return (targetloc, val)

def getSum(node, curSum, res):
    loc, val = node[0], node[1]
    left = find(loc, "left")
    right = find(loc, "right")
    hasnode = val!=sys.maxsize
    hasleft = left[1]!=sys.maxsize
    hasright = right[1]!=sys.maxsize

    if not hasnode:
        res[0]+=curSum
        return
    if not hasleft and not hasright:
        curSum+=val
        res[0]+=curSum
        return
    if hasleft:
        getSum(left, curSum+val, res)
    if hasright:
        getSum(right, curSum+val, res)
    return


treeDict = defaultdict()
for num in nums:
    treeDict[num//10] = num%10


root = (11, nums[0]%10)
res=[0]
getSum(root, 0, res)
return res[0]

```



### 132. Palindrome Partitioning II

Hard

Given a string `s`, partition `s` such that every substring of the partition is a palindrome.

Return *the minimum cuts needed* for a palindrome partitioning of `s`.

 

**Example 1:**

```
Input: s = "aab"
Output: 1
Explanation: The palindrome partitioning ["aa","b"] could be produced using 1 cut.
```

**Example 2:**

```
Input: s = "a"
Output: 0
```

**Example 3:**

```
Input: s = "ab"
Output: 1
```

 

+ DFS: TLE

  ```python
  def isPalindrome(s):
      if len(s)<=1: return True
      l,r = 0, len(s)-1
      while l<r:
          if s[l]!=s[r]: return False
          l+=1
          r-=1
      return True
  
  def partition(res, s, start, path):
      if start==len(s):
          res[0] = min(res[0],path-1)
          return
  
      for i in range(start+1,len(s)+1):
          cur = s[start:i]
          if isPalindrome(cur):
              partition(res,s,i,path+1)
      return
  
  res=[len(s)-1]
  partition(res,s,0,0)
  return res[0]
  ```

+ DFS with memorization, TLE

  ```python
  def isPalindrome(s):
      if s in palindromes: return palindromes[s]
      if len(s)<=1: return True
      l,r = 0, len(s)-1
      while l<r:
          if s[l]!=s[r]: 
              palindromes[s] = False
              return False
          l+=1
          r-=1
      palindromes[s] = True
      return True
  
  def partition(s, start):
      if start in memo:
          return memo[start]
  
      if start==len(s):
          return -1
  
      minPath = sys.maxsize
      for i in range(start+1,len(s)+1):
          cur = s[start:i]
          if isPalindrome(cur):
              minPath = min(1+partition(s,i), minPath)
      memo[start] = minPath
      return minPath
  
  palindromes = defaultdict()
  memo = defaultdict()
  return partition(s, 0)
  ```

  

+ DP solution:

```python
class Solution:
    def minCut(self, s: str) -> int:
        n = len(s)
        dp = [i - 1 for i in range(0, n + 1)] # dp[i]: the minimum cuts needed for s[:i]
        for i in range(0, n + 1):
            j = 0 # odd
            while i - j >= 0 and i + j < n and s[i + j] == s[i - j]:
                dp[i + j + 1] = min(dp[i + j + 1], dp[i - j] + 1)
                j += 1
            j = 1 # even
            while i - j + 1 >= 0 and i + j < n and s[i - j + 1] == s[i + j]:
                dp[i + j + 1] = min(dp[i + j + 1], dp[i - j + 1] + 1)
                j += 1
        return dp[n] 
```



### 518. Coin Change 2

Medium

You are given coins of different denominations and a total amount of money. Write a function to compute the number of combinations that make up that amount. You may assume that you have infinite number of each kind of coin.



 

**Example 1:**

```
Input: amount = 5, coins = [1, 2, 5]
Output: 4
Explanation: there are four ways to make up the amount:
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1
```

**Example 2:**

```
Input: amount = 3, coins = [2]
Output: 0
Explanation: the amount of 3 cannot be made up just with coins of 2.
```

**Example 3:**

```
Input: amount = 10, coins = [10] 
Output: 1
```



+ DP with amount*numCoins

```python
dp = [0]*(amount+1)
dp[0] = 1
for coin in coins:
    for i in range(amount+1):
        start = i
        if i-coin>=0:
            dp[i]+=dp[i-coin]     
return dp[-1]
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

 

+ Store words in dict:
  + `{*ot:[hot, dot], h*t:[hot, hit], ho*:[hot]}`
  + `hit: h*t-> hot: *ot-> dot: do*->dog: *og->cog`



```python
    if not beginWord or not endWord or not wordList: return 0
    wordsDict = defaultdict(list)
    for word in wordList:
        for i in range(len(word)):
            key = word[:i] + '*' + word[i + 1:]
            wordsDict[key].append(word)

    queue = deque([[beginWord, 1]])

    visited = set()
    while queue:
        cur, curLength = queue.popleft()
        for i in range(len(cur)):
            key = cur[:i] + '*' + cur[i + 1:]
            for nextWord in wordsDict[key]:
                if nextWord in visited:
                    continue
                if nextWord==endWord:
                    return curLength+1
                queue.append([nextWord, curLength+1])
                visited.add(nextWord)

    return 0
```



### 317. Shortest Distance from All Buildings

Hard

You want to build a house on an *empty* land which reaches all buildings in the shortest amount of distance. You can only move **up, down, left and right**. You are given a 2D grid of values **0**, **1** or **2**, where:

- Each **0** marks an empty land which you can pass by freely.
- Each **1** marks a building which you cannot pass through.
- Each **2** marks an obstacle which you cannot pass through.

**Example:**

```
Input: [[1,0,2,0,1],[0,0,0,0,0],[0,0,1,0,0]]

1 - 0 - 2 - 0 - 1
|   |   |   |   |
0 - 0 - 0 - 0 - 0
|   |   |   |   |
0 - 0 - 1 - 0 - 0

Output: 7 

Explanation: Given three buildings at (0,0), (0,4), (2,2), and an obstacle at (0,2),
             the point (1,2) is an ideal empty land to build a house, as the total 
             travel distance of 3+3+1=7 is minimal. So return 7.
```

**Note:**
There will be at least one building. If it is not possible to build such house according to the above rules, return -1.



+ Input:  2D array
  + illegal: 1,2,. legal: 0
+ output:
  + a location of 0 which can reach all 1 in shortest total distance
+ BFS from each 1
  + add cur distance to each zero

```python
import sys
from collections import defaultdict
class Solution:
    def shortestDistance(self, grid: List[List[int]]) -> int:
        
       
        if not grid: return -1
        rows, cols = len(grid), len(grid[0])
        minDist = [sys.maxsize]
        ones = defaultdict()
        numones = 1
        
        for i in range(rows):
            for j in range(cols):
                if grid[i][j]==1:
                    ones[(i,j)]=numones
                    numones+=1
        
        numones-=1
        
        dists = defaultdict()
        accessibleOnes = defaultdict()
        
        di = [1,0,-1,0]
        dj = [0,1,0,-1]
        
        
        for one, numOne in ones.items():
            if not one: continue
            
            visited = set()
            queue = deque([(one,0)])
            while queue:
                cur, curDist = queue.popleft()
                for k in range(4):
                    i,j = cur[0]+di[k], cur[1]+dj[k]
                    if 0<=i<rows and 0<=j<cols and grid[i][j]==0 and (i,j) not in visited:
                        visited.add((i,j))
                        if (i,j) not in dists:
                            dists[(i,j)] = curDist+1
                            accessibleOnes[(i,j)] = 1
                        else:
                            dists[(i,j)]+= curDist+1
                            accessibleOnes[(i,j)] += 1
                        queue.append(((i,j),curDist+1))
        
            
        minDist = sys.maxsize
        for key, dist in dists.items():
            if accessibleOnes[key]==numones:
                minDist = min(minDist, dist)
        
        if minDist == sys.maxsize:
            return -1
        
        return minDist
                        
            
```





### 993. Cousins in Binary Tree

Easy

In a binary tree, the root node is at depth `0`, and children of each depth `k` node are at depth `k+1`.

Two nodes of a binary tree are *cousins* if they have the same depth, but have **different parents**.

We are given the `root` of a binary tree with unique values, and the values `x` and `y` of two different nodes in the tree.

Return `true` if and only if the nodes corresponding to the values `x` and `y` are cousins.

 

**Example 1:
![img](https://assets.leetcode.com/uploads/2019/02/12/q1248-01.png)**

```
Input: root = [1,2,3,4], x = 4, y = 3
Output: false
```

**Example 2:
![img](https://assets.leetcode.com/uploads/2019/02/12/q1248-02.png)**

```
Input: root = [1,2,3,null,4,null,5], x = 5, y = 4
Output: true
```

**Example 3:**

**![img](https://assets.leetcode.com/uploads/2019/02/13/q1248-03.png)**

```
Input: root = [1,2,3,null,4], x = 2, y = 3
Output: false
```

 

```python
    queue = deque([(root, 0)])
    targetDepth = -1
    queueCount = 0

    while queue:
        cur, curDepth = queue.popleft()
        if not cur: continue
        if cur.val in [x,y]:
            if targetDepth == -1:
                targetDepth = curDepth
            else:
                return targetDepth == curDepth

        if cur.left and cur.right and cur.left.val in [x,y] and cur.right.val in [x,y]:
            return False
        queue.append((cur.left, curDepth+1))
        queue.append((cur.right, curDepth+1))

    return False
```





### 103. Binary Tree Zigzag Level Order Traversal

Medium

2992120Add to ListShare

Given a binary tree, return the *zigzag level order* traversal of its nodes' values. (ie, from left to right, then right to left for the next level and alternate between).

For example:
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



+ Deque alternating direction

  ```python
  from collections import deque
  
  class Solution:
      def zigzagLevelOrder(self, root: TreeNode) -> List[List[int]]:
          res = []
          if not root: return
          queue = deque([root])
          even = True
          while queue:
              res.append([node.val for node in list(queue)])
              size = len(queue)
              curQueue = deque()
              while size>0:
                  if even:
                      cur = queue.popleft()
                      if cur.left:
                          curQueue.appendleft(cur.left)
                      if cur.right:
                          curQueue.appendleft(cur.right)
                  else:
                      cur = queue.pop()
                      if cur.left:
                          curQueue.append(cur.left)
                      if cur.right:
                          curQueue.append(cur.right)
                  size-=1
                  
              queue = curQueue
              
              even = not even
          return res
  ```

  + Stack

  ```python
  class Solution:
      def zigzagLevelOrder(self, root: TreeNode) -> List[List[int]]:
          if not root: return
          stack = [root]
          res = []
          even = True
          while stack:
              res.append([node.val for node in stack])
              stack2 = []
              while stack:
                  cur = stack.pop()
                  if not even:
                      if cur.left:
                          stack2.append(cur.left)
                      if cur.right:
                          stack2.append(cur.right)
                  else:
                      if cur.right:
                          stack2.append(cur.right)           
                      if cur.left:
                          stack2.append(cur.left)
              stack = stack2
              even = not even
          return res
  ```

  



### 513. Find Bottom Left Tree Value

Medium

Given the `root` of a binary tree, return the leftmost value in the last row of the tree.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/12/14/tree1.jpg)

```
Input: root = [2,1,3]
Output: 1
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/12/14/tree2.jpg)

```
Input: root = [1,2,3,4,null,5,6,null,null,7]
Output: 7
```

+ DFS with level record, Look at first at each level

```python
from collections import deque

class Solution:
    def findBottomLeftValue(self, root: TreeNode) -> int:
        if not root: return root
        queue = deque([root])
        left = root.val
        while queue:
            size = len(queue)
            while size:
                size-=1
                
                cur = queue.popleft()
                if cur.left:
                    queue.append(cur.left)
                if cur.right:
                    queue.append(cur.right)
            if queue:
                left = queue[0].val
        
        return left
```



### 505. The Maze II

Medium

There is a **ball** in a maze with empty spaces and walls. The ball can go through empty spaces by rolling **up**, **down**, **left** or **right**, but it won't stop rolling until hitting a wall. When the ball stops, it could choose the next direction.

Given the ball's **start position**, the **destination** and the **maze**, find the shortest distance for the ball to stop at the destination. The distance is defined by the number of **empty spaces** traveled by the ball from the start position (excluded) to the destination (included). If the ball cannot stop at the destination, return -1.

The maze is represented by a binary 2D array. 1 means the wall and 0 means the empty space. You may assume that the borders of the maze are all walls. The start and destination coordinates are represented by row and column indexes.

 

**Example 1:**

```
Input 1: a maze represented by a 2D array

0 0 1 0 0
0 0 0 0 0
0 0 0 1 0
1 1 0 1 1
0 0 0 0 0

Input 2: start coordinate (rowStart, colStart) = (0, 4)
Input 3: destination coordinate (rowDest, colDest) = (4, 4)

Output: 12

Explanation: One shortest way is : left -> down -> left -> down -> right -> down -> right.
             The total distance is 1 + 1 + 3 + 1 + 2 + 2 + 2 = 12.
```

**Example 2:**

```
Input 1: a maze represented by a 2D array

0 0 1 0 0
0 0 0 0 0
0 0 0 1 0
1 1 0 1 1
0 0 0 0 0

Input 2: start coordinate (rowStart, colStart) = (0, 4)
Input 3: destination coordinate (rowDest, colDest) = (3, 2)

Output: -1

Explanation: There is no way for the ball to stop at the destination.
```

 

**Note:**

1. There is only one ball and one destination in the maze.
2. Both the ball and the destination exist on an empty space, and they will not be at the same position initially.
3. The given maze does not contain border (like the red rectangle in the example pictures), but you could assume the border of the maze are all walls.
4. The maze contains at least 2 empty spaces, and both the width and height of the maze won't exceed 100.



+ Dijkstra's algorithm

```python
class Solution:
    def shortestDistance(self, maze: List[List[int]], start: List[int], destination: List[int]) -> int:
        if not maze: return -1
        if start == destination: return 0
          
        rows, cols = len(maze), len(maze[0])
        
        distance = [[sys.maxsize for _ in range(cols)] for _ in range(rows)]
        
        queue = deque([start])
        distance[start[0]][start[1]]=0
        
        di = [1,0,-1,0]
        dj = [0,1,0,-1]
        
        
        while queue:
            cur = queue.popleft()
            for k in range(4):
                i,j = cur
                dist = distance[i][j]
                while 0<=i<rows and 0<=j<cols and maze[i][j]==0:
                    i += di[k]
                    j += dj[k]
                    dist += 1
                i -= di[k]
                j -= dj[k]
                dist -= 1
                
                if distance[i][j] > dist:
                    distance[i][j]=dist
                    if i != destination[0] or j != destination[1]:
                        queue.append((i,j))
        
        res = distance[destination[0]][destination[1]]
        return res if res != sys.maxsize else -1
```



### 499. The Maze III

Hard

There is a **ball** in a maze with empty spaces and walls. The ball can go through empty spaces by rolling **up** (u), **down** (d), **left** (l) or **right** (r), but it won't stop rolling until hitting a wall. When the ball stops, it could choose the next direction. There is also a **hole** in this maze. The ball will drop into the hole if it rolls on to the hole.

Given the **ball position**, the **hole position** and the **maze**, find out how the ball could drop into the hole by moving the **shortest distance**. The distance is defined by the number of **empty spaces** traveled by the ball from the start position (excluded) to the hole (included). Output the moving **directions** by using 'u', 'd', 'l' and 'r'. Since there could be several different shortest ways, you should output the **lexicographically smallest** way. If the ball cannot reach the hole, output "impossible".

The maze is represented by a binary 2D array. 1 means the wall and 0 means the empty space. You may assume that the borders of the maze are all walls. The ball and the hole coordinates are represented by row and column indexes.

 

**Example 1:**

```
Input 1: a maze represented by a 2D array

0 0 0 0 0
1 1 0 0 1
0 0 0 0 0
0 1 0 0 1
0 1 0 0 0

Input 2: ball coordinate (rowBall, colBall) = (4, 3)
Input 3: hole coordinate (rowHole, colHole) = (0, 1)

Output: "lul"

Explanation: There are two shortest ways for the ball to drop into the hole.
The first way is left -> up -> left, represented by "lul".
The second way is up -> left, represented by 'ul'.
Both ways have shortest distance 6, but the first way is lexicographically smaller because 'l' < 'u'. So the output is "lul".
```

**Example 2:**

```
Input 1: a maze represented by a 2D array

0 0 0 0 0
1 1 0 0 1
0 0 0 0 0
0 1 0 0 1
0 1 0 0 0

Input 2: ball coordinate (rowBall, colBall) = (4, 3)
Input 3: hole coordinate (rowHole, colHole) = (3, 0)

Output: "impossible"

Explanation: The ball cannot reach the hole.
```

 





```python
import sys
from collections import deque

class Solution:
    def findShortestWay(self, maze: List[List[int]], ball: List[int], hole: List[int]) -> str:
        start = ball
        destination = hole
        if not maze: return -1
        if start == hole: return 0
        rows, cols = len(maze), len(maze[0])
        distance = [[sys.maxsize for _ in range(cols)] for _ in range(rows)]
        path = [['' for _ in range(cols)] for _ in range(rows)]
        
        queue = deque([start])
        distance[start[0]][start[1]] = 0
        
        di = [1,0,-1,0]
        dj = [0,1,0,-1]
        directions = ['d','r','u','l']
        
        
        while queue:
            cur = queue.popleft()
            for k in range(4):
                i,j = cur
                dist = distance[i][j]
                curPath = path[i][j]

                while 0<=i<rows and 0<=j<cols and maze[i][j]==0 and (i != hole[0] or j != hole[1]):
                    i += di[k]
                    j += dj[k]
                    dist += 1

     
                if i != hole[0] or j != hole[1]:
                    i -= di[k]
                    j -= dj[k]
                    dist -= 1
                
                # print(i,j,distance[i][j], dist)
                newPath = curPath+directions[k] 
                if distance[i][j] > dist or (distance[i][j] == dist and path[i][j]> newPath):
                    distance[i][j]=dist
                    path[i][j] = newPath
                    if i != hole[0] or j != hole[1]:
                        queue.append((i,j))
                        
        res = distance[destination[0]][destination[1]]
        return path[destination[0]][destination[1]] if res != sys.maxsize else 'impossible'
```

