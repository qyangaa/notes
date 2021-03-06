

### Math

```python
import math
math.ceil(p/k) #equivalent to:
((p-1) // k) + 1 #two times faster
math.floor(p/k) #equivalent to:
p//k

# check if is perfect square
def is_square(i: int) -> bool:
    return i == math.isqrt(i) ** 2

# check if is prime: go until sqrt(n) with step 2
def is_prime(n):
    return all(n % i for i in range(2, sqrt(n))) # for not overflow n
    return n > 1 and all(n % i for i in islice(count(2), int(sqrt(n)-1))) #count from 2 to int(...)

# count number of primes numbers < n (204)
isPrime = [True]*n
count  = 0
for i in range(2, n):
    if not isPrime[i]: continue
    j = 2
    count += 1
    while i*j < n:
        isPrime[i*j] = False
        j += 1
return count
```

#### Centroid in grid (by Manhattan Distance)

```python
h, w = len(G), len(G[0])

I = [i for i in range(h) for j in range(w) if G[i][j] == 1]
J = [j for j in range(w) for i in range(h) if G[i][j] == 1]

median_pos = len(I) // 2
mi, mj = I[median_pos], J[median_pos]

return sum(abs(x-mi) for x in I) + sum(abs(x-mj) for x in J)
```

#### Series

+ Geometric series: $\sum ar^{n-1} = a\frac{1-r^n}{1-r}$ 

#### Sum problems

+ Subarray sum multiple of k: ```if prefixSum[i]%k in seen(prefixSum[:i]%k)```

+ Sum of rectangle in grid: prefix sum `br - (bl + tr - tl)`

### max min

```python
import sys
max = sys.maxsize
min = -sys.maxsize - 1
max = float('inf')
min = float('-inf')
```

### String

```python
s.isalpha() #if is letter
s.isalnum() #determin if is alphanumeric
s.lower() #to lower case

# Operate on characters of string
s = list(S)
Operations...
S = ''.join(s)
' '.join(map(str, res)) # list -> str
```



### List/ Array

```python
None in list  # check is non is in list
all(v is None for v in l) # check if all items in list is none
l.pop(idx)
# max in 2d grid
row_maxes = [max(row) for row in grid]
col_maxes = [max(col) for col in zip(*grid)]
```

### Hash

```python
hash(val) # generate a hash value
```



### Set

```python
set2 & set1 # intersection
A.intersection(*other_sets) # intersection destroy
set1 | set2 # union
A.union(*other_sets) # union destroy
set2 - set1 # asymetric difference
```



### Reverse Range:

```python
for i in reversed(range(k)):
```



### Counter

```python
# remove negative and zero counters
c = collections.Counter(a=-2, b=0, c=3)
c += collections.Counter()
```





### Count then Sort by Count

```python
import collections
import heapq


counter.most_common()
count = list(collections.Counter(items).items())
#sort by count (max) then alphabetical
count.sort(key=lambda x: (-x[1], x[0]))
#heap to find top k count (logk time)
heapq.nlargest(k,count, lambda x: (-x[1], x[0]))
```



### Sort

```python
counter.sort(key = lambda x: (x[1], x[0]), reverse=False)
```



### Python Heapq

+ Only supports minHeap
+ Use `-value` for maxheap. 

```python
import heapq
list=[]
heap = heapq.heapify(list)
heapq.heappush(heap, item)
item = heapq.heappop()
size=len(heap)
peakItem = heap[0]
heapq.heappush(heap, (l.val,id(l), l)) # use id() if need to push non-comparable object
heapq.nsmallest(n, iterable, key=None)
```



### Bisect

```python
bisect.bisect_left(a, x, lo=0, hi=len(a))
bisect.bisect_right(a, x, lo=0, hi=len(a))
bisect.bisect(a, x, lo=0, hi=len(a))
bisect.insort_left(a, x, lo=0, hi=len(a))
bisect.insort_right(a, x, lo=0, hi=len(a))
bisect.insort(a, x, lo=0, hi=len(a))


import bisect
n = [1,2,3,4,4,5,6]
# first >=k
print(bisect.bisect_left(n,4))  # 3
print(bisect.bisect_left(n,3)) # 2
print(bisect.bisect_left(n,3.5)) # 3
# first > k 
print(bisect.bisect_right(n,4)) # 5
print(bisect.bisect_right(n,3)) # 3
print(bisect.bisect_right(n,3.5)) # 3

bisect_left:
def search(n, k):
    l,r = 0, len(n)
    while l<r:
        m = l+(r-l)//2
        if n[m]>=k:#smallest idx satisfies this
            r=m
        else:
            l=m+1
    return l
```



### Permutation and combination

```python
from itertools import permutations  
perm = list(permutations([1, 2, 3]))

from itertools import combinations 
comb = list(combinations([1, 2, 3], 2))
```



### Functools

```python
@functools.cache
def dfs(i, j): ...
```





## Two Pointer

+ Application
  + Sorted array/ linked list
  + To find two (set of) numbers subject to some conditions

### Start and end pointer

```python
A.sort()
first, second = 0, len(A)-1
while first<second:
    if earlyStop: return result
    if small: first+=1
    if big: second-=1
```

#### Examples

+ Pivot sort

  ```python
  while first<second:
      while first<second and nums[first]<=p: first+=1
      while first<second and nums[second]>=p: second-=1
      nums[first],nums[second]=nums[second],nums[first]
      first+=1
      second+=1
  ```

## Sliding Window

+ Optimize when substring has legal counts: 3, 159, 340, 495, 424, 567, 1004, 1100, 1151

```python
count = defaultdict(int)
l = 0
res = 1
for r in range(len(s)):
    count[s[r]] += 1
    while illegal:
        count[s[l]] -= 1
        l += 1
    res = max(res, r-l+1)
return res
```

+ Optimize (sum) for fixed length substring: 1052, 1176, 1208, 1423

```python
for r, cur in enumerate(nums):
    curSum += cur
    if r >= k-1:
        minSum = min(curSum, minSum)
        curSum -= nums[r-k+1]
```





#### Monotonic Queue

+ [1696, 1499, 1438, 1425](https://zxi.mytechroad.com/blog/tag/monotonic-queue/)

+ 239 Sliding Window Maximum

```python
class MonoQ:
    def __init__(self):
        self.key = deque()
    def push(self,key, value):
        while self.key and key > self.key[-1]: self.key.pop()
        self.key.append(key)
    def pop(self):
        return self.key.popleft()
    def max(self):
        return self.key[0]
    def isEmpty(self):
        return len(self.key) == 0

# solution
q = MonotonicQueue()
res = []
for i in range(len(nums)):
    q.push(nums[i])
    if i-k+1 >=0: # get to window size
        res.push(q.max())
        if nums[i-k+1] == q.max(): q.pop() # pop only if start of queue is current number
return res
```

+ 1438 Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit

```python
min_q = deque()
max_q = deque()
l = 0
maxLen = 0
for r, cur in enumerate(nums):
    while min_q and cur < min_q[-1]:
        min_q.pop()
    while max_q and cur > max_q[-1]:
        max_q.pop()
    min_q.append(cur)
    max_q.append(cur)
    while max_q[0] - min_q[0]  > limit:
        if max_q[0] == nums[l]: max_q.popleft()
        if min_q[0] == nums[l]: min_q.popleft()
        l += 1
    maxLen = max(maxLen, r-l+1)
return maxLen
```

+ 739: min(r-l), nums[r] > nums[l]: decreasing queue
+ 901: consequtive items before current with value <= current : decreasing queue
+ 862: min(r-l), nums[r] - nums[l] > K:  increasing queue, pop left

+ 907: Sum of Subarray Minimums

  + PLE: distance to previous smaller number

    ```python
            stack = []
            # get PLE: increasing stack
            for i in range(n):
                while stack and arr[i]<arr[stack[-1]]:
                    stack.pop()
                if not stack:
                    left[i] = i + 1
                else:
                    left[i] = i - stack[-1]
                stack.append(i)
    ```

  + NLE: distance to next smaller number

    ```python
            # get NLE: decreasing stack
            stack = []
            for i in range(n-1, -1, -1):
                while stack and arr[i]<=arr[stack[-1]]:
                    stack.pop()
                if not stack:
                    right[i] = n - i 
                else:
                    right[i] = stack[-1] - i
                stack.append(i) 
    ```

  + Number of subarrays with minimum number at i: ```left[i] * right[i]```

+ 975: Odd Even Jump

  ```python
  class Solution:
      def oddEvenJumps(self, arr: List[int]) -> int:
          n = len(arr)
          next_higher, next_lower = [0]*n, [0]*n
          
          # Find smallest next higher and largest next lower elements
          stack = [] # decreasing stack
          for a, i in sorted([a,i] for i, a in enumerate(arr)):
              while stack and stack[-1]<i:
                  next_higher[stack.pop()] = i
              stack.append(i)
          stack = []
          for a, i in sorted([-a,i] for i, a in enumerate(arr)):
              while stack and stack[-1]<i:
                  next_lower[stack.pop()] = i
              stack.append(i)
          
          # DP
          higher, lower = [0]*n, [0]*n
          higher[-1] = lower[-1] = 1
          for i in range(n-2, -1, -1):
              higher[i] = lower[next_higher[i]]
              lower[i] = higher[next_lower[i]]
          return sum(higher)
  ```

+ 1696 Jump Game VI: Max sum subsequence with distances between indexes <=k, DP + decreasing queue

  ```python
  class Solution:
      def maxResult(self, nums: List[int], k: int) -> int:
          n = len(nums)
          q = deque([0])
          dp = [0]*n
          dp[0] = nums[0]
          for i in range(1, n):
              dp[i] = nums[i] + dp[q[0]]
              while q and dp[i] >= dp[q[-1]]:
                  q.pop()
              while q and i-q[0] >=k:
                  q.popleft()
              q.append(i)
          return dp[n-1]
              
  ```

  

## Sort

### $O(n^2)$ Algorithms

```python
def bubbleSort(nums):
    for i in range(len(nums)):
        for j in range(1,len(nums)-i):
            if nums[i]>nums[j]:
                nums[i], nums[j] = nums[j], nums[i]
# best O(n)
def bubbleSort(nums):
    for i in range(len(nums)):   
        swapped=False
        for j in range(1,len(nums)-i):
            if nums[i]>nums[j]:
                nums[i], nums[j] = nums[j], nums[i]
                swapped=True
        if not swapped: break
```

```python
def insertSort(nums):
    for i in range(1, len(nums)):
        for j in reversed(range(1,i+1)):
            if nums[j-1]>nums[j]:
                nums[j-1],nums[j]=nums[j],nums[j-1]
# best O(n)                
def insertSort(nums):
    for i in range(1, len(nums)):
        temp = nums[i]
        for j in reversed(range(1,i+1)):
            if temp<nums[j-1]
                nums[j] = nums[j-1]     
            else:
                break
        nums[j] = temp
```

```python
def selectSort(nums):
    for i in range(len(nums)-1):
        minVal = nums[i]
        minIdx = i
        for j in range(i+1,len(nums)):
            if nums[j]<minVal:
                minVal=nums[j]
                minIdx=j
        nums[minIdx] = nums[i]
        nums[i]=minVal
```



### $O(n\log n)$ Algorithms

+ Merge Sort

```python
def mergeSort(nums, l, r):
    if l<r:
        m = l+(r-l)//2
        mergeSort(nums,l,m)
        mergSort(nums,m+1,r)
        merge(nums,l,m,r)
def merge(nums,l,m,r):
    i,j = l,m+1
    res=[]
    while i<=m and j<=r:
        if nums[i]<=nums[j]:
            res.append(nums[i])
            i+=1
        else:
            res.append(nums[j])
            j+=1
    while i<=m:
        res.append(nums[i])
        i+=1
    while j<=r:
        res.append(nums[j])
        j+=1
    return res
```

Question: merge in place $O(n)$ time?

+ https://stackoverflow.com/questions/2571049/how-to-sort-in-place-using-the-merge-sort-algorithm

```python
void wmerge(Key* xs, int i, int m, int j, int n, int w) {
    while (i < m && j < n)
        swap(xs, w++, xs[i] < xs[j] ? i++ : j++);
    while (i < m)
        swap(xs, w++, i++);
    while (j < n)
        swap(xs, w++, j++);
}  
```



+ QuickSort

```python
def quickSort(nums, begin, end):
    if begin>end: return
    pivot = partition(nums, begin, end)
    quickSort(nums,begin,pivot-1)
    quickSort(nums, pivot+1, end)
def partition(nums, begin, end):
    pivot=nums[begin]
    while begin<end:
        while begin<end and nums[end]>=pivot:
            end-=1
        nums[begin] = nums[end]
        while begin<end and nums[begin]<=pivot:
            begin+=1
       	nums[end] = nums[begin]
    nums[begin] = pivot
    return begin
```



### Bucket Sort

+ $O(max(n,range))$ , good when length of array is much larger than range of the numbers

```python
count = [0]*numRange
for num in nums:
    count[num]+=1
res=[]
for i, val in enumerate(count):
    res+=[i]*val
```





## Linked List

+ Get, set, add(idx), remove(idx): $O(n)$

+ Head is special case (use dummy head)

  + ```python
    dummy = ListNode(-1)
    dummy = head
    pre = dummy
    ... return dummy.next
    ```

+ Tips

  + Check whether `head==null`
  + Check null when using while (`node!=None`for all `node.next` to access)
    + `node.next?: node.next.next`
  + draw out movement of (fast-slow) pointers and calculate relationship between their path length
  + Check assumptions about: size, index range, tail

```python
class ListNode:
    def __init__(self, val_):
        self.val = val_
        self.next = None

def add(head, idx, value):
    dummy = ListNode(-1)
    dummy.next = head
    pre = dummy
    while idx>0:
        if not pre: return ERROR
        pre=pre.next
    node = ListNode(value)
    head, pre.next, node.next =dummy.next, node, pre.next

def length(head):
    res = 0
    cur=head
    while cur:
        cur=cur.next
        res+=1
    return length

def goto(head, index):
    cur = head
    while index>0:
        if not cur: return ERROR
        cur=cur.next
        index-=1
    return cur
```

### Type 1: get length then find

+ k'th node from the end

  ```PYTHON
  length = length(head)
  index = length-k
  goto(head, index)
  ```

### Type 2: fast-slow pointers

+ k'th node from the end

  ```python
  first = head
  while k>0:
      first=first.next
  second = head
  while first:
      first=first.next
      second=second.next
  return second
  ```

+ Middle node: test even length and odd length, should end at `idx = (len-1)//2`

  ```python
  fast=slow=head
  while fast.next and fast.next.next: # check both
      fast = fast.next.next
      slow = slow.next
  return slow
  ```

+ detect cycle

  ```python
  if not head: return False
  fast=slow=head
  while fast:
      if not fast.next: return False # checked here
      if fast.next==slow: return True
      fast = fast.next.next
      slow = slow.next
  ```

+ Find cycle entrance

  ```python
  fast=slow=head
  while fast: # slow-fast until meet
      if not fast.next: return False
      if slow==fast: break
      slow=slow.next
      fast = fast.next.next
  fast = head
  while slow!=fast: # slow-slow until meet
      slow = slow.next
      fast = fast.next
  return fast
  ```

  

### List Modification

+ If need remove, stand at pre, look at pre.next and pre.next.next

+ remove duplicates

  ```python
  if not head: return head
  pre = head
  while pre.next:
      if pre.val==pre.next.val:
          pre.next = pre.next.next
      else:
          pre = pre.next
  return head
  ```

+ reverse list: use a cyclic method: `pre; cur-> cur.next` becomes `pre<-cur; cur.next` then renames to `..<-pre; cur`

  ```mermaid
  graph LR;
   cur-->pre;
   pre --> cur.next;
   cur.next-->cur;
  
  ```

  ```mermaid
  
  ```

  

  ```python
  pre=None # have to start from None, else list have cycle
  cur = head
  while cur!=None:
      pre, cur.next, cur = cur, pre, cur.next
  return pre
  ```

+ Delete node: If head is not accessible, we can only delete next node, not current node

  ```python
  if not node: return
  if not node.next: 
      node=null
      return
  node.val = node.next.val
  node.next = node.next.next
  ```



### Merge Linked List

21, 148,1634

```python
dummy = Node()
c, c1, c2 = dummy, head1, head2
while c1 and c2:
    if c1.val<c2.val:
        node = Node(c1.val)
        c1 = c1.next
    else:
        node = Node(c2.val)
        c2 = c2.next
    c.next = node
    c = c.next
c.next = c1 or c2 #easy way to deal with remaining
return dummy.next
```




## Stack

+ Valid parantheses

```python
pair = []
n = s.length()
if n==0: return True
left2right = {'(':')','[':']','{':'}'}
for i in range(n):
    if s[i]==in left2right.keys(): 
        pair.append(s[i])
    else:
        last = pair.pop(-1)
        if left2right[s[i]]!=last: return False
return len(pair)==0 # important to check if there are pars left
```



## Recursion

### Summary

+ 1 subproblem: Factorial, sum of Linked List, remove linked list element
+ OR(subproblems): Maze, Sudoku, DFS
+ AND(subproblems): Knapsack, permutations, combinations, eight queen print, kSum

### Vanilla Recursion (to find one path)

```python
def recursion(matrix, index, target, path):
    if not isLegal(matrix, index) or inPath(index, path): return False # or return []
    if isTarget(index, target): return True # or return path
    
    for nextIdx in directions:
        path.add(index)
        if recursion(matrix, nextIdx, target, path): return True # or return path
        path.remove(index)
    return False
```

### Recursion with backtracking (to find all paths)

```python
def recursion(matrix, index, results): # results: list of results
    if baseCase: #index == len(matrix)-1
        saveResult(matrix, results)
        return
    for nextIdx in indexes:
        if isLegal(matrix, nextIdx):
            mark(nextIdx)
            recursion(matrix, nextIndex, results) #results passed as reference
            unmark(nextIdx)
    return results            
```



### Recursion with memory

```python
memo={}
def helper(i,j,..):
    if base_case(i,j): return
    if (i,j) not in memo:
        memo[(i,j)] =...
    return memo[(i,j)]
return helper(n,m)
```

### Basic Examples

+ greatest common divisor: 

  ```python
  def gcd(x,y): (x>y)
      if y==0: return x
      return gcd(y, x%y)
  ```

+ Climb building: 1 or 2 stairs a time, print all ways

  ```python
  def climb(n, preWay):
      if n==1: 
          print(preWay+" 1")
      	return
      if n==2:
          print(preWay+" 2")
          print(preWay+" 1 1")
          return
      preWay1 = preWay+" 1"
      climb(n-1,preWay1)
      preWay2 = preWay+" 2"
      climb(n-1,preWay2)
  ```

+ Towers of Hanoi: (n-1) from A to B, n'th from A to C, (n-1) from B to C

  ```python
  # number of steps
  def hanoi(n):
      if n==1: return 1
      return 2*hanoi(n-1)+1
  # Print all steps
  def hanoi(n, start, mid, end):
      if n==1:
          print(Start+ "->"+end)
          return
      hanoi(n-1, start, end, mid)
      print(Start+ "->"+ end)
      hanoi(n-1, mid, start, end)
  ```

+ Remove element from Linked List

  ```python
  if not head: return head
  newHead = remove(head.next, val)
  if head.val==val:
      return newHead
  else:
      head.next=newHead
      return head
  ```

  

+ Reverse Linked List

  ```python
  if not head or not head.next: return head
  newHead = reverse(head.next)
  head.next.next = head # point next to current
  head.next = null
  return newHead
  ```

+ 



### Permutation

```python
def permute(idx, prevPath, res):
    if idx==len(s):
        res.append(prevPath)
    
    for c in s: # The only difference from combination: search for all, and check if used
        if c in prevPath: continue  
        permute(idx+1, prevPath+c, res)
```



### Combination

+ Sorting is required to prevent duplication for list with duplications

```python
def combination(idx, prevPath, res):
    if len(prevPath)==num: # length of returning list
        res.append(prevPath.copy())
        return
    if idx>=n: #length of provided number range
        return
    # if use current or not
    combination(idx+1, prevPath+[idx], res)
    combination(idx+1, prevPath,res)
    return
```

```python
def combination(idx, prevPath, res):
    if idx==len(s) or baseCase():
        res.append(prevPath)
        return

    for nextIdx in nextSteps: # items to the right, or current children, e.g. range(idx, len(items)), must include current idx
        cur = items[nextIdx]
        combination(nextIdx+1, prevPath+cur, res)
    return

path=[] # or ""
res=[]
combination(0, path, res)
return res
```

```python
# Backtracking, can be slower, but saves some memory (may not be much)
def combination(idx, prevPath, res):
    if idx==len(s) or baseCase():
        res.append(prevPath.copy())
        return

    for nextIdx in nextSteps:
        cur = items[nextIdx]
        prevPath.append(cur) # doesn't work for string
        combination(nextIdx+1, prevPath, res)
        prevPath.pop()
    return
```



### 0-1 Knapsack

```python
def knapsack(weights, target, idx):
    if target==0: return True
    if target<0 or idx>=len(weight): return False
    return knapsack(weights, target-weights[idx], idx+1) or 
knapsack(weights, target idx+1)
```

### Maze

```python
# See if can reach
def dfs(maze, start, end,visited):
    if start==end: return True
    if illegal(start): return False
    visited.add(start)
    for nextIdx in directions:
        if dfs(maze, nextIdx, end, visited):
            return True
    return False

# Print Path
def dfs(maze, start, end,prevPath, visited):
    if start==end: 
        print(prevPath)
        return True
    if illegal(start): return False
    visited.add(start)
    for nextIdx in directions:
        if dfs(maze, nextIdx, end, prevPath+maze[nextIdx], visited):
            return True
    return False
```



### N queens

```python
def dfs(row, path, res):
    if row==n:
        savePath(path, res)
        return
    for col in range(n):
        if isLegal(row, col, path):
            path.append(col)
            dfs(row+1, path, res)
            del path[-1]
    return

def savePath(path, res):
    cur = []
    for loc in path:
        cur.append('.'*loc+'Q'+'.'*(n-loc-1))
    res.append(cur)
    return

def isLegal(row, col, path):
    for i in range(row):
        if path[i]==col or abs(path[i]-col)==abs(i-row):
            return False
    return True

path=[]
res=[]
dfs(0,path,res)
return res
```



## Recursion - DP

#### Subset Sum

+ true or false:

  + Recursion

  ```python
  def subsetSum(nums, target, index):
      if target==0: return True
      if index>=len(nums): return False
      cur = target - nums[index]
      return subsetSum(nums, target-cur, index+1) or subsetSum(nums, target, index+1)
  ```

  + DP: `[1,2,4],5`

    + -> remaining
    + v: use numbers until index
    + content: whether has sum

    |        | 0    | 1    | 2    | 3              | 4    | 5             |
    | ------ | ---- | ---- | ---- | -------------- | ---- | ------------- |
    | 0      | T    | F    | F    | F              | F    | F             |
    | 1`[1]` | T    | T    | F    | F              | F    | F             |
    | 2`[2]` | T    | T    | T    | (`[1][3-1]`) T | F    | F             |
    | 3`[5]` | T    | T    | T    | T              | F    | (`[2][5-5]`)T |

    

  ```python
  def subsetSum(nums, target):  
      dp = [[False for _ in range(target)]for _ in range(nums)]
      dp[0][:]=[True for _ in range(target)]
      dp[:][0]=[False for _ in range(nums)]
      for index in range(1,nums):
          for curTarget in range(1,target):
              cur = nums[index]
              if curTarget >= cur:
                  dp[index][target]=dp[index-1][target] or dp[index-1][target-cur]
              else:
                  dp[index][target]=dp[index-1][target]
      return dp[-1][-1]
      
      
  # Reduce space use
  def subsetSum(nums, target):  
      dp = [False for _ in range(target)]
      dp[0] = True
      for index in range(1,nums):
          for curTarget in range(1,target):
              cur = nums[index]
              if curTarget >= cur:
                  dp[target]=dp[target] or dp[target-cur]
              else:
                  dp[index][target]=dp[target]
      return dp[-1]
  ```

  

+ Save Paths:

  + Recursion

  ```python
  def subsetSum(nums, target, index=0, path=[], res):
      if not nums or index==len(nums):
          return
      if target==0:
          res.append(path.copy())
      	return
      cur = nums[index]
      if target>cur:
          path.append(cur)
          subsetSum(target-cur, index+1, path, res)
          path.pop()
      subsetSum(target, index+1, path, res)
  	return    
  ```

  



## DP

```python
memo=[]
memo[0][0]=... #init base cases
for i ..:
    for j ..:
        memo[i][j]=... #transition
return memo[n][m]
```

### DP cases

### 1D, depend on $O(1)$ sub problem

Max sum subarray:

```python
dp[i] = max(dp[i-1]+arr[i], arr[i])
```



198. [740] House Robber (max sum of no adjacent house), 

```python
for i in range(N):
    dp[i][0] = dp[i-1][1]+val[i] # rob
    dp[i][1] = max(dp[i-1][0], dp[i-1][1]) # no rob
return max(dp[N][0], dp[N][1])
```

123 Best Time to Buy and Sell Stock III: two (by+sell) only

![image-20210202081942691](/home/arkyyang/files/notes/notes/attachments/image-20210202081942691.png)

```python
dp[0][0] = dp[0][1]= dp[0][2] = dp[0][3] = -prices[0]
for i in range(N):
    dp[i][0] = max(dp[i-1][0], -val[i]) #have 1
    dp[i][1] = max(dp[i-1][1], dp[i-1][0]+val[i]) # no 1
    dp[i][2] = max(dp[i-1][2], dp[i-1][1]-val[i]) #have 2
    dp[i][3] = max(dp[i-1][3], dp[i-1][2]+val[i]) #no 2
return max(max(dp[-1]),0)
```

309 Best Time to Buy and Sell Stock with Cooldown: must cooldown for 1 day after sell before buy

```python
dp[0][0] = -prices[0]
dp[0][1] = float('-inf')
for i in range(1, n):
    dp[i][0] = max(dp[i-1][0], dp[i-1][2] - val[i]) # has
    dp[i][1] = dp[i-1][0] + val[i]# sell
    dp[i][2] = max(dp[i-1][2], dp[i-1][1])# wait with no stock
return max(dp[-1][1], dp[-1][2])
```

376 Wiggle subsequence

![image-20210202091343180](/home/arkyyang/files/notes/notes/attachments/image-20210202091343180.png)

```python
up[0] = down[0] = 1;
for i in range(1,n):
    if nums[i] < nums[i-1]:
        down[i] = up[i-1]+1
    else:
        down[i] = down[i-1]
    if nums[i] > nums[i-1]:
        up[i]  = down[i-1] + 1
    else:
        up[i] = up[i-1]
return max(up[-1], down[-1])        
```

1289  Minimum Falling Path Sum II:
exactly one element from each row of `arr`, such that no two elements chosen in adjacent rows are in the same column.

```python
for i in range(1,n):
    for j in range(m):
        dp[i][j] = min([dp[i-1][k] for k in range(m) if k!=j ]) + arr[i][j]
return min(dp[-1])
```



#### To Do or Not to Do

+ Two states: do, not

487 Max Consecutive Ones II

 binary array, find the maximum number of consecutive 1s in this array if you can flip at most one 0.

```python
noFlip[0] = nums[0]
flip[0] = 1
for i in range(1,n):
    if nums[i] == 1:
        noFlip[i] = noFlip[i-1]+1
        flip[i] = flip[i-1] + 1
    else:
        noFlip[i] = 0
        flip[i] = noFlip[i-1]+1
return max(flip)
```

1186 Maximum Subarray Sum with One Deletion

maximum sum for a **non-empty** subarray (contiguous elements) with at most one element deletion

```python
for i in range(1, n):
    de[i] = max(no[i-1], de[i-1]+arr[i])
    no[i] = max(no[i-1]+arr[i], arr[i])
return max(max(de), max(no))
```





### 1D, depend on $O(n)$ subproblem

+ Related to problem [0,i-1] (sum, max, min)

300 Longest Increasing Subsequence

```python
for i in range(1,n):
    dp[i] = 1;
    for j in range(0,i):
        if nums[j]<nums[i]:
        	dp[i] = max(dp[i], dp[j]+1)
return max(dp)

# With bisect
maxList = [nums[0]] + [float('inf')]*len(nums)
maxIdx = 0
for num in nums[1:]:
    idxl = bisect_left(maxList, num)
    idxr = bisect_right(maxList, num)
    if idxl == idxr: # key not present
        maxIdx = max(maxIdx, idxl)
        maxList[idxl] = min(maxList[idxl], num)
return maxIdx + 1
```

673 Number-of-Longest-Increasing-Subsequence

```python
dp = [1]*n
count = [1]*n
for i in range(1,n):
    for j in range(0,i):
        if nums[j]<nums[i]:
            if dp[i] == dp[j]+1:
                count[i] += count[j]
            elif dp[j]+1 > dp[i]:
                dp[i] = dp[j]+1
                count[i] = count[j]
return sum([count[i] for i in range(n) if dp[i]==max(dp)])
```

368 Largest Divisible Subset

```python
nums.sort()
for i in range(1,n):
    for j in range(0,i):
        if nums[i]%nums[j] == 0:
            dp[i] = max(dp[i], dp[j]+1)
            if dp[i] == dp[j]+1:
                sets[i] = sets[j] + [nums[i]]
    if dp[i]> maxVal:
        maxVal, maxIdx = dp[i], i
return sets[maxIdx]
```

1105 Filling bookcase shelves

Group S into subarrays, minimize the sum(max(subarray)), with sum(widths) of each subarray <= W

dp[i]: min sum(max(subarray)) of S[:i] 

``` python
for i in range(1, n):
    for j in range(i-1, -1):
        if totWidth[j+1,i]<=W:
            dp[i] = min(dp[i], dp[j]+ maxHeight[j+1,i])
        else:
            break
return dp[-1]
```

+ 297 perfect squares: use square numbers as subproblems instead of all numbers < i

```python
for i in range(1, n):
    for square in [i**2 for i in range(int(sqrt(n)) + 1)]:
        if i<square: break
        dp[i] = min(dp[i], dp[i-square] + 1)
return dp[-1]
```

+ 264: n'th Ugly Number  (positive number whose prime factors only include `2`, `3`, and/or `5`), DP with subproblem pointers to prevent traverse all subproblems:

```python
nums = [1]
i2, i3, i5 = 0, 0, 0
for i in range(1, n):
    cur = min(nums[i2]*2, nums[i3]*3, nums[i5]*5)
    nums.append(cur)
    if cur == nums[i2]*2: i2+=1
    if cur == nums[i3]*3: i3+=1    
    if cur == nums[i5]*5: i5+=1
return nums[-1]
```



+ Others: 96, 

#### 1D, 2 sets of sub-problems

845 Longest Mountain in Array

```python
for i in range(len(A)):
    if A[i]>A[i-1] inc[i]=inc[i-1]+1
for i in reversed(range(len(A))):
    if A[i]<A[i-1] dec[i]=dec[i-1]+1 
ans=0
for i in range(len(A)):
    if inc[i] and dec[i]: 
        ans=max(ans, inc[i]+dec[i]+1)
```

926 Flip String to Monotone Increasing [TODO]

```python
for i in range(n):
    l[i]=l[i-1]+S[i]-'0'
for i in reversed(range(n-1)):
    r[i]=r[i+1]+'1'-S[i]
ans=r[0]
for i in range(n):
    ans=min(ans, l[i-1]+r[i])
```

#### 1D, multiple states

### 2D

+ 718 Longest Common Subarray

```python
# dp[i][j]: LCS between first i items of l1 and first j items of l2
if A[i] == B[j]: dp[i][j] = dp[i-1][j-1] + 1
```

+ 1143 Longest Common Subsequence

```python
# dp[i][j]: LCS between first i items of l1 and first j items of l2
if l1[i-1] == l2[j-1]: dp[i][j] = dp[i-1][j-1] + 1
else: dp[i][j] = max(dp[i-1][j], dp[i][j-1])
```

+ 1092 Shortest Common Supersequence

```python
# Find length
for i in range(1,m):
    for j in range(1,n):
        if s[i]==t[j]:
            dp[i][j] = dp[i-1][j-1]+1
        else:
            dp[i][j] = min(dp[i-1][j]+1, dp[i][j-1]+1)
```

```python
# Find sequence
dp: from LCS
if i == 0: 
    s.appendLeft(l2[j-1])
    j -= 1
elif j == 0:
    s.appendLeft(l1[i-1])
    i -= 1
if l1[i-1]==l2[j-1]:
    s.appendLeft(l1[i-1])
    i -= 1
    j -= 1
elif dp[i][j] == dp[i-1][j]:
    s.appendLeft(l1[i-1])
    i -= 1
elif dp[i][j] == dp[i][j-1]:
    s.appendLeft(l2[j-1])
    j -= 1
```

+ 72 Min Edit Distance: num insert, delete, replace to make two words the same

```python
# need padding before
dp[0] = [i for i in range(m+1)]
for i in range(n+1):
    dp[i][0] = i

for i in range(1,n+1):
    for j in range(1,m+1):
        if word1[i-1]==word2[j-1]:
            dp[i][j] = dp[i-1][j-1]
        else:
            dp[i][j] = min(dp[i-1][j], dp[i-1][j-1],dp[i][j-1])+1
```

+ 97 Interleaving sting![img](https://assets.leetcode.com/uploads/2020/09/02/interleave.jpg)

```python
# padding required
for i in range(1, n+1):
    if s1[i-1] == s3[i-1]:
        dp[i][0] = True
    else:
        break

for i in range(1, m+1):
    if s2[i-1] == s3[i-1]:
        dp[0][i] = True
    else:
        break



for i in range(1,n+1):
    for j in range(1,m+1):

        if (dp[i-1][j] and s1[i-1] == s3[i+j-1]) or (dp[i][j-1] and s2[j-1] == s3[i+j-1]):
            dp[i][j] = True

return dp[-1][-1]              
            
```

+ 115 Distinct subsequence

```python
for i in range(1,n):
    dp[i][0] = dp[i-1][0]
    if s[i] == t[0]:
        dp[i][0] += 1

for i in range(1,n):
    for j in range(1,m):
        if s[i] == t[j]:
            dp[i][j] = dp[i-1][j-1] + dp[i-1][j]
        else:
            dp[i][j] = dp[i-1][j]
```

+ 727 Minimum Window Subsequence

minimum (contiguous) **substring** `W` of `S`, so that `T` is a **subsequence** of `W`.

```python
dp[i][j]: min length substring ending with S[i] with subsequence T[:j]
if S[0]==T[0]:
    dp[0][0] = 1
for i in range(1,n):
    if S[i] == T[0]:
        dp[i][0] = 1
    else:
        dp[i][0] = dp[i-1][0]+1
    
for i in range(1,n):
    for j in range(1,m):
        if S[i] == T[j]:
            dp[i][j] = dp[i-1][j-1] + 1
        else:
            dp[i][j] = dp[i-1][j] + 1
return min(dp[:][m-1])
```



#### 4. separate into K consecutive windows

<img src="/home/arkyyang/files/notes/notes/attachments/image-20210205081553687.png" alt="image-20210205081553687" style="zoom:70%;" />

1278 Palindrome Partitioning III 

min number of change character to divide into k palindrome substrings

```python
for i in range(1, n):
    for k in range(1, K):
        for j in range(i, k-1, -1):
            dp[i][k] = min(dp[i][k], dp[j-1][k-1] + change(j,i))
return dp[-1][K]
```

813 Largest Sum of Averages

Max sum of averages of at most K groups

```python
dp[i][k] = max(dp[i][k], dp[j-1][k-1] + avg(j,i))
```

410 Split Array Largest Sum

Min largest sum among m subarrays

```python
dp[i][k] = min(dp[i][k], max(dp[j-1][k-1], sum(j,i)))
```

1335 Minimum Difficulty of a job schedule

Min sum (max (val of each subarray))

```python
curMax = 0
for j in range(i, k-1, -1):
    curMax = max(curMax, A[j])
    dp[i][k] = min(dp[i][k], dp[j-1][k-1]+ curMax)
```



#### 5. one sequence, depends on smaller size

<img src="/home/arkyyang/files/notes/notes/attachments/image-20210205094433187.png" alt="image-20210205094433187" style="zoom:67%;" />

l: length, i: start, calculate end j from i+l-1, scan over middle point k

+ 516 Longest Palindromic subsequence

```python
dp[i][j]: longest palindromic subsequence from i to j
for l in range(1,n):
    for i in range(0, n-l):
        j  = i + l - 1
    if l == 1:
        dp[i][j]
    if s[i] == s[j]:
        s[i][j] = s[i+1][j-1] + 2
    else:
        s[i][j] = max(s[i+1][j], s[i][j-1])
```

+ 312 Burst Ballons: max result

```python
for i in range(n-2, -1 ,-1):
    for j in range(i+2, n):
        dp[i][j] = max(nums[i]*nums[k]*nums[j] + dp[i][k] + dp[k][j] for k in range(i+1, j))
```

+ 375 Guess number higher or lower II

min tot cost to guarantee win guess in range number 1 to n

```python
minlen = 2
for i in range(n-(minlen-1), -1, -1):
    for j in range(i+(minlen-1), n):
        dp[i][j] = min(k + max(dp[i][k-1], dp[k+1][j]) for k in range(i,j+1))




dp = [[0]*(n+2) for _ in range(n+2)]

for l in range(2,n+1):
    for i in range(1,n-l+2):
        j = i+l-1
        dp[i][j] = float('inf')
        for k in range(i, j+1):
            dp[i][j] = min(dp[i][j], k+max(dp[i][k-1], dp[k+1][j]))

return dp[1][n]
```

+ 1246 Palindrome Removal

min number of palindrome removals to remove all numbers

```python
for i in range(n, -1, -1):
    for j in range(i, n):
        if i == j :
            dp[i][j] = 1
        	continue
        dp[i][j] = min(dp[i][k-1]+max(1, dp[k+1][j-1]) for k in range(i,j+1) and s[k]==s[j])

for l in range(1, n+1):
    for i in range(1, n-l+2):
        j = i+l-1
        for k in range(i, j+1):
            if s[k]==s[j]:
            	dp[i][j] = min(dp[i][j], dp[i][k-1]+max(1, dp[k+1][j-1]))
```

#### 6. Knapsack

Can choose whether take i'th item or not.

Standard 0-1 knapsack

```python
dp[0][0] = 0
dp[0][c] = Min
for i in range(1,N):
    for c in range(1,C):
        dp[i][c] = max(dp[i-1][c], dp[i-1][c-wi]+vi)
```

494 Target Sum

```python
for i in range(1,n):
    for c in range(-maxSum, maxSum+1):
        dp[i][s] = dp[i-1][s-nums[i]] + dp[i-1][s+nums[i]];
```

1049 Last Stone Weight II

```python
for i in range(1,n):
    for s in range(-maxSum, maxSum+1):
        dp[i][s] = dp[i-1][s-nums[i]] or dp[i-1][s+nums[i]]
return first positive s s.t. dp[N][s] is true
```

474 Ones and zeros

input: array of binary strings.

return: size of largest subset with at most m 0's and n 1's

```python
for i in range(1,N+1):
    for cm in range(1,m+1):
        for cn in range(1,n+1):
            if cm-num0[i-1]>=0and cn-num1[i-1]>=0:
                dp[i][cm][cn] = max(dp[i-1][cm-num0[i-1]][cn-num1[i-1]]+1, dp[i-1][cm][cn])
```



#### grid

+ [1277, 221] squares in matrix

```python
# max square with bottom right vertice at i,j
dp[i][j] = min(dp[i][j-1], dp[i-1][j], dp[i-1][j-1]) + 1
# sum number of all squares:
res += dp[i][j]
```

+ [304] sum of rectangles: 2D prefix sum 

```python
dp[i+1][j+1] = grid[i][j] + dp[i][j+1] + dp[i+1][j] - dp[i][j]
```



## Greedy

### Coverage

+ 45, 1326: Jump game, iterate over maxReach, doesn't require sorting, require dense step (every i has value)

```python
#  maxReach[i] = max idx reachable from idx i
steps = l = e = 0
for i in range(n+1):
    if i>e: return -1
    if i>l:
        steps += 1
        l = e # need to start a new tap from current pos
    e = max(e, maxReach[i]) # max reachable by current tap
return steps
```

+ 1024, 1326: video stitching, find max end for every start < curEnd, require sorted [(begin, end)..]

```python
ranges = [(begin, end)...].sort()
steps = i = l = e = 0
while e<n:
    while i <= n and ranges[i] <= l: # when start < curEnd
        e = max(e, t[i][1]) # find max end
        i += 1
   	l = e # update curMaxReach
    steps += 1
return steps
```





## Search

### Binary Search

+ Time complexity: $O(log(r-1)*(f(m)+g(m)))$
+ returns left most element where g(m) holds
+ Upper bound: `m=m=l+(r-l)//2;l=m+1 `
+ Lower bound: `m=m=l+(r-l+1)//2;r=m-1`

```python
### Upper bound: right most index of target + 1
# l: smallest idx>val
# l-1: largest idx <= val
l = 0, r = len(A) # open right
    while l<r:
        m=l+(r-l)//2
        if A[m]>val: #smallest index satifies this inequality
            r=m
        else: l=m+1
    return l

### Lower bound: left most index of target (inclusive)
# l: smallest idx >=val
# l-1: largest idx<val
l = 0, r = len(A)
    while l<r:
        m=l+(r-l)//2
        if A[m]>=val:
            r=m
        else: l=m+1
    return l
```

#### Examples - simple g(m)

+ 69  `floor(sqrt(x))`

  ```python
  def f(x):
      l=0
      r = x+1
      while l<r:
          m=l+(r-l)//2
          if m>x/m: #l**2>x
              r=m
          else:
              l=m+1
      return l-1
  ```

+ 278 First Bad Version

  ```python
  def f(x):
      l=0
      r=n
      while l<r:
          m=l+(r-l)//2
          if isBadVersion(m):
              r=m
          else: l=m+1
      return l
  ```

+ Search in rotated array: good to draw

  <img src="/home/arkyyang/files/notes/notes/attachments/image-20210118205856339.png" alt="image-20210118205856339" style="zoom:50%;" />

  ```python
  m = l+(r-l)//2
  if A[l]>A[m]:
      if A[m]<target<A[r]: l=m+1
      else: r=m
  else:
      if A[m]>target>A[l]: r=m
      else: l=m+1
  return A[l]==target
  ```

+ sqrt

  ```python
  def sqrt(x): #floor (biggest number <= result)
      if x<=1: return x
      l, r = 0, x//2+1
      while l<r:
          m = l+(r-l+1)//2 #here we want the lower bound(floor)
          if m*m==x: return m
          if m*m<x:l=m
          else: r=m-1
      return l
  ```

  

#### Examples - complex g(m)

[TODO]







## DFS & BFS

### Grid

```python
if not A return False
n,m = len(A),len(A[0])
ans;
for i in range(n):
    for j in range(m):
        ans = f(ans, DFS(i,j))
return ans

def DFS(i,j):
    if illegal(i,j): return False
    if earlyStop(grid[i][j]): return
    if base_case(i,j): return #base case, usually gives True
    cur = grid[i][j]
    grid[i][j]=None #prevent reuse
    res = LOGIC(DFS(d) for d in directions) #(i-1,j)(i+1,j)(i,j-1)(i,j+1)
    grid[i][j]=cur #put back if backtracking needed
    return res

def BFS(i,j):
    seen=set()
    queue = deque((i,j))
    while queue:
        r,c = queue.popleft()
        for d in [(r+1,c),(r-1,c),(r,c+1),(r,c-1)]:
            if illegal(d): 
                seen.add(d) #prevent reuse
                continue
            if earlyStop(d): return result
            seen.add(d) # add to visited when appending not when popping
            queue.append(d)
    return NOT_FOUND

def illegal(i,j):
    if i<0 or j<0 or i>=n or j>=m: return True
    if (i,j) in seen: return True
    if notAvailable(i,j): return True #blocked grid
    return False

def earlyStop(i,j):
    if (i,j) = target: return True
```





##### Example

+ 79 word search in grid - DFS with backtracking

  ```python
  n = len(board)
  m = len(board[0])
  for i in range(n):
      for j in range(m):
          if DFS(board, word, 0, i,j) return True
  return False
  
  def DFS(board, word, d, i,j):
      if i<0 or i>= n or j<0 or j>=m: return False
      if word[d] != board[i][j] return False
  	if d==len(word)-1: return True
      cur = board[i][j]
      board[i][j]=0 #prevent reuse
      found = DFS(board, word,d+1, i+1,j) 
      		or DFS(board, word,d+1, i-1,j) 
          	or DFS(board, word,d+1, i,j+1) 
              or DFS(board, word,d+1, i,j-1)
      board[i][j]=cur
      return found    
  ```

+ 200 number of islands - no backtracking

  ```python
  if not grid: return 0
  m, n = len(grid), len(grid[0])
  ans = 0
  for i in range(n):
      for j in range(m):
          if grid[i][j]=='1':
              ans+=1
              dfs(grid,i,j,m,n)
  return ans
  
  def dfs(A,i,j,m,n):
      if illegal(A,i,j,m,n): return
      A[i][j]='0'
      dfs(A,i+1,j,m,n)
      dfs(A,i,j+1,m,n)
      dfs(A,i-1,j,m,n)
      dfs(A,i,j-1,m,n)    
      
  def illegal(A,i,j,m,n):
      if i<0 or j<0 or i>=m or j>=m: return True
      if A[i][j]=='0': return True
      return False
      
  ```

+ 542 01 matrix BFS

  ```python
  n,m=len(M),len(M[0])
  queue=deque((i,j,0) for i in range(n) for j in range(m) if M[i][j]==0)
  ans=[[0 for _ in range(m)] for _ in range(n)]
  while queue:
      r,c,d = queue.popleft()
      for dr,dc in [(0,1),(0,-1),(-1,0),(1,0)]:
          i,j=r+dr, j+dj
          if 0<=i<n and 0<=j<m and matrix[i][j]==1:
              matrix[i][j]=0
              queue.append((i,j,d+1))
  return ans
          
  ```

+ 37 sudoku solver

  ```python
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
              if board[i][j]==".": return i,j
      return -1,-1
  
  def canSolve():
      i,j=findVacant()
      if i==-1 and j==-1: return True #no more vacancy
      
      for num in range(1,10):
          if islegal(i,j,str(num)):
              board[i][j]=str(num)
              if canSolve(): return True
              board[i][j]='.' #backtrack
      return False
  
  canSolve()
              
  ```

  

### Combination/ Permutation

```python
def DFS(n,item,result):
    if illegal: return
    if found: result.append(item)
    for cur in curChoices:
        DFS(n-1,item+cur,result)
    return

def BFS(items):
    if not items: return []
    for item in items:
        temp = []
        for cur in curChoices:
            temp.append(item+cur)
        ans = temp
   return ans

def permute(nums):
    if len(nums)<=1: return [nums]
    res=[]
    for i,num in enumerate(nums): #pick one to start
        for right in permute(nums[0:i]+nums[i+1:]): #recurse on all remaining nums
            res.append([num]+right)
    return res


def combine(items, n):
    if n==1: return [[item] for item in items]
    if len(items)<n: return[]
    res=[]
    for i,item in enumerate(items):
        for right in combine(items[i+1:],n-1):
            res.append([item]+right)
    return res
```

##### Examples

+ 22 Generate parentheses (n pairs)

  ```python
  dfs(l,r,item,result):
      if r<l: return #illegal parantheses
      if l==0 and r==0: result.append(item) #legal final result
      if l>0: dfs(l-1,r,item+'(',result)
      if r>0: dfs(l,r-1,item+')',result)
  
  if n==0:
      return []
  result=[]
  dfs(n,n,'',result)
  return result
  ```

  



## Binary Tree

### Basics

+ Depth (of node): # edges to root
+ Height: longest path to leaf
+ Types:
  + ![image-20210118200855665](/home/arkyyang/files/notes/notes/attachments/image-20210118200855665.png)

+ Properties:
  + at most $2^i$ nodes at level $i$
  + at most $ceil(2^{k+1})-1$ nodes for height $k$ tree
  + Complete tree: height $\log_2{(n+1)}-1$ for $n$ nodes
  + parent index $k$, children $2k+1, 2k+2$
  + Perfect binary tree: $n = 2^{h}-1$, root height = 1
+ Traversal:
  + BFS: LevelOrder
  + DFS: pre/ in/ post  order

### LevelOrder Traversal

```python
queue=deque([root])
while queue:
    cur = queue.popleft()
    f(cur)
    if cur.left: queue.append(cur.left)
    if cur.right: queue.append(cur.right)
```

<img src="/home/arkyyang/files/notes/notes/attachments/image-20210118201529133.png" alt="image-20210118201529133" style="zoom:25%;" />

#### LevelOrder record level

```python
### two queues: empty a queue when finish a level
queue=deque([root])
level=0
while queue:
    level+=1
    queue2 = deque()
    while queue:
        cur = queue.popleft()
        f(cur)
        if cur.left: queue2.append(cur.left) #new level to new queue
    	if cur.right: queue2.append(cur.right)
    queue=queue2
    
### add null at end of each level
queue=deque([root,None])
level=0
while queue:
    cur = queue.popleft()
    if not cur: #level ends
        level+=1
        queue.append(None)
    else:
        f(cur)
        if cur.left: queue.append(cur.left)
        if cur.right: queue.append(cur.right)
            
### count size
queue=deque([root])
level, curLevelNum=0,1
while queue:
    level+=1
    nextLevelNum=0
    while curLevelNum!=0:
        curLevelNum-=1
        cur = queue.popleft()
        f(cur)
        if cur.left: queue.append(cur.left), nextLevelNum++1
        if cur.right: queue.append(cur.right), nextLevelNum++1
    curLevelNum = nextLevelNum
```

### DFS

#### Seialization/ Deserialization

+ Serialization: 652(Inorder), 606(preorder)
+ Deserialization: 536

```python
# Binary Tree
def serialize(root):
    if not root: return "^"
    return str(root.val) + "," + serialize(root.left) + "," + serialize(root.right)
def deserialize(data):
    data = deque(data.split(","))
    def build(data):
        cur = data.popleft()
        if cur == "^": return None
        node = TreeNode(int(cur))
        node.left = build(data)
        node.right = build(data)
        return node
    return build(data)

# N-ary Tree
def serialize(self, root: 'Node') -> str:
    if not root: return ""
    res = str(root.val)
    res += "," + str(len(root.children)) # store number of children right after root value
    for child in root.children:
        res += "," + self.serialize(child)
    return res

def deserialize(self, data: str) -> 'Node':
    if not data: return None
    data = deque(data.split(","))
    def build(data):
        cur = int(data.popleft())
        num = int(data.popleft())
        node = Node(cur)
        node.children = []
        for i in range(num):
            node.children.append(build(data))
        return node
    return build(data)


# Binary tree paranthesis
def preorderSerialize(node):
    if not node: return "#"
    serial = "{},{},{}".format(node.val, preorderSerialize(node.left), preorderSerialize(node.right))
    return serial
def preorderDeserialize(s): #s = "4(2(3)(1))(6(5))"
    for ch in s:
        if ch in "()":
            if ch == '(' and num: # if see (, push
                stack.append(TreeNode(num))
                num = ''
            elif ch == ")": # if see ), pop and connect to parent 
                if num: # parent is always the prev item on stack
                    node, parent = TreeNode(num), stack[-1]
                    num = ''
                else: # if no num, process item in stack
                    node = stack.pop()
                    parent = stack[-1]

                if parent.left:
                    parent.right = node
                else:
                    parent.left = node
        else:
            num += ch

    if num:
        stack = [TreeNode(num)]
    return stack[0]
```

#### One root

```python
def f(root):
    if not root: return
    if base_case(root): return #(eg. if not root.left and not root.right)
    l = f(root.left)
    r=f(root.right)
    return g(root, l, r)
```

#### Iterative Methods

```python
# Preorder: visit, push right, push left
stack = [root]
while stack:
    cur = stack.pop()
    visit(cur)
    if cur.right: stack.append(cur.right) # right first then left
    if cur.left: stack.append(cur.left)

# Preorder: save space by pushing only right
stack = [root]
cur = root
while stack:
    visit(cur)
    if cur.right: stack.append(cur.right)
    if cur.left: 
        cur = cur.left
    else:
        cur = stack.pop()
visit(cur)  

# Inoder: push, go left, if no left, pop, visit, go right
# Inorder Binary Tree: 94, 173
# Inorder Successor in BST: 285 (no parent pointer, give root, p), 510 (has parent pointer, give p)
stack = []
cur = root
while stack or cur:
    if cur:
        stack.append(cur)
        cur = cur.left
    else:
        cur = stack.pop()
        visit(cur)
        cur = cur.right

# Postorder:
# Indirect method: Reverse append and left/right of pre-order method use deque
stack = [root]
cur = root
q = deque([])
if not root: return []
while stack:
    q.appendleft(cur.val)
    if cur.left: stack.append(cur.left)
    if cur.right:
        cur = cur.right
    else:
        cur = stack.pop()
return list(q)
# Direct method:
# go down until leaf, process leaf and pop stack
# go up from left node, if has right push, else go up and pop
res = []
stack = [root]
prev = None
while stack:
    cur = stack[-1]
    # search for leaf
    if not prev or prev.left == cur or prev.right == cur:
        if cur.left:
            stack.append(cur.left)
        elif cur.right:
            stack.append(cur.right)
        else: # leaf
            stack.pop()
            res.append(cur.val)
    elif cur.left == prev:
        if cur.right:
            stack.push(cur.right)
        else:
            stack.pop()
            res.append(cur.val)
    elif cur.right == prev:
        stack.pop()
        res.append(cur.val)
    rev = cur
return res
```

#### Iterative Deserialize

```python


# Postorder [1628]
stack = []
for c in postfix:
    if c.isdigit(): # isleaf
        stack.append(TreeNode(int(c)))
    else:
        r = stack.pop()
        l = stack.pop()
        stack.append(TreeNode(c, l, r))
return stack[-1]
```





#### Root-to-leaf sum

```python
def maxRoot2LeafSum(start):
    maxDist = -1
    for nei in graph[start]:
        maxDist = max(maxDist, maxRoot2LeafSum(nei) + value(start, nei))
    if maxDist == -1: return 0
    return maxDist
```



#### Find Path

##### Recursion

```python
def findPath(root, path, res):
    if not root: return
    path += root.val
    if not root.left and not root.right: 
        res.append(path)
    else:
        findPath(root.left, path, res)
        findPath(root.right, path, res)
path, res=[], []
findPath(root, path, res)
return res
```

##### Iteration

```python
if not root: return []
res, stack = [], [[root, [root.val]]]
while stack:
    cur, path = stack.pop()
    if not cur.left and not cur.right:
        res.append(path)
    if cur.left:
        stack.append([cur.left, path+[cur.left.val]])
    if cur.right:
        stack.append([cur.right, path+[cur.right.val]])
return res
```





##### Examples

+ 104 max depth

  ```python
  def maxDepth(root):
      if not root: return 0
      l = maxDepth(root.left)
      r = maxDepth(root.right)
      return max(l,r)+1
  ```

+ 111 min depth (root-leaf)

  ```python
  def minDepth(root):
      if not root: return 0
      if not root.left and not root.right: return 1 #leaf
      l = minDepth(root.left)
      r = minDepth(root.right)
      if not root.left: return 1+r
      if not root.right: return 1+l
      return min(l,r)+1
  
  ### or:
  def minDepth(root):
      if not root: return 0
      if not root.left and not root.right: return 1
      if root.left and root.right: 
          return min(minDepth(root.left),minDepth(root.right))+1
      if root.left: return minDepth(root.left)+1
      if root.right: return minDepth(root.right)+1
  
  ### Faster with BFS
  if not root: return 0
  queue=deque([root])
  minDepth=0
  while queue:
      minDepth+=1
      queue2 = deque()
      while queue:
          cur = queue.popleft()
          if not cur.left and not cur.right:
              return minDepth
          if cur.left: queue.append(cur.left)
          if cur.right: queue.append(cur.right)
      queue=queue2
  return minDepth
  ```

+ 112 Path Sum

  ```python
  def pathSum(root, sum):
      if not root: return false
      if not root.left and not root.right: return root.val==sum
      l = pathSum(root.left, sum-root.val)
      r = pathSum(root.right, sum-root.val)
      return l or r
  
  ### Print results
  def pathSum(root, sum, path, results):
      if not root: return
      if not root.left and not root.right: 
          if root.val==sum:
              path.append(root.val)
              results.append(path.copy())
              path.pop()
      	return
      path.append(root.val)
      pathSum(root.left, sum-root.val, path, results)
      pathSum(root.right, sum-root.val, path, results)
      path.pop()
  ```

+ 687 Longest Univalue Path

#### Lowest Common Ancestor

+ I: 236 only children pointer, given root, p, q, guarantee exist

  II: 1644 only children pointer, given root, p, q, not** guarantee exist
  
  1740: min distance between two nodes

```python        def recurse_tree(current_node):
res = [None]
def recurse_tree(current_node):
    if not current_node:
        return False
    left = recurse_tree(current_node.left)
    right = recurse_tree(current_node.right)
    mid = current_node == p or current_node == q
    if mid + left + right >= 2:
        res[0] = current_node
    return mid or left or right

recurse_tree(root)
return res[0]
```

+ III: 1650 has parent pointer, given p, q, guarantee exist

```python
containsq = set([p.val])
while p.parent:
    p = p.parent
    containsq.add(p.val)

if q.val in containsq: return q
while q.parent:
    q = q.parent
    if q.val in containsq: break

return q
```

+ IV: 1676 only children pointer, given root, list of nodes

```python
if not root:
    return None
if root in nodes:
    return root
left, right = self.lowestCommonAncestor(root.left, nodes), self.lowestCommonAncestor(root.right, nodes)
if not left:
    return right
if not right:
    return left
else: 
    return root
```

+ 235 BST, given root, p, q  (is common ancestor if cur val in between two nodes)

```python
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

+ 865, 1123: LCA of deepest nodes

```python
def depth(node): # return (depth, cur LCA of deepest)
    if not node: return (0, None)
    left, right = depth(node.left), depth(node.right)
    if left[0] > right[0]:
        return (left[0]+1, left[1])
    elif left[0] < right[0]:
        return (right[0]+1, right[1])
    else:
        return (left[0]+1, node)
return depth(root)[1]
```



#### Flip Tree

+ 156: upside down: .left->root; root->.right; .right->.left, 1666(description unclear)

```python
    if not root: return root
    node = root
    while node.left:
        node = node.left
    self.flip(root)
    return node

def flip(self, root):
    if not root or not root.left: 
        return root
    newRoot = self.flip(root.left)
    newRoot.right = root
    root.left = None        
    newRoot.left = self.flip(root.right)
    root.right = None

    return root
```



#### Two roots

```python
def f(p, q):
    if not p and not q: return
    if base_case(p,q): return #(eg: if not p or not q)
    l = f(p.child, q.child)
    r = f(p.child, q.child)
    return g(p,q,l,r)
```

##### Examples

+ 100 Same Tree

  ```python
  def f(p,q):
      if not p and not q: return True
      if not p or not q: return False
      l = f(p.left,q.left)
      r = f(p.right, q.right)
      return p.val==q.val and l and r
  ```

+ 101 Symmetric Tree

  ```python
  def f(p,q):
      if not p and not q: return True
      if not p or not q: return False
      l = f(p.left, q.right)
      r = f(p.right, q.left)
      return p.val==q.val and l and r
  ```

+ 951 Flip equivalent binary trees (can either flip or not flip)

  ```python
  def f(p,q):
      if not p and not q: return True
      if not p or not q: return False
      l1 = f(p.left, q.left)
      l2 = f(p.left, q.right)
      r1 = f(p.right, q.right)
      r2 = f(p.right, q.left)
      return p.val==q.val and ((l1 and r1) or (l2 and r2))
      
  ```

#### All unique trees

+ 96 Unique BST with values 1-n, 95 list all unique BSTs: 

```python
# Count
G = [0]*(n+1)
G[0], G[1] = 1, 1

for i in range(2, n+1):
    for j in range(1, i+1):
        G[i] += G[j-1] * G[i-j]

return G[n]

# List
res = []
memo = {}

def bindRoot(rootVal: int, leftChildren: List[TreeNode], rightChildren: List[TreeNode]):
    if not leftChildren and not rightChildren: return [TreeNode(rootVal)]
    curList = []
    for lchild in leftChildren:
        for rchild in rightChildren:
            root = TreeNode(rootVal)
            root.left = lchild
            root.right = rchild
            curList.append(root)
    return curList

def helper(l, r):
    if l>r: return [None]
    if (l,r) in memo: return memo[(l,r)]
    curList = []
    for i in range(l, r+1):
        curList += bindRoot(i, helper(l, i-1), helper(i+1, r))
    memo[(l, r)] = curList
    return curList

helper(1, n)
return memo[(1,n)]
```

+ 894 all full binary trees with n nodes

```python
if n%2 == 0: return []
memo = {}
memo[0] = None
memo[1] = [TreeNode(0)]

def bindRoot(leftChildren, rightChildren):
    if not leftChildren and not rightChildren: return [TreeNode(0)]
    if not leftChildren or not rightChildren: return []
    curList = []
    for lchild in leftChildren:
        for rchild in rightChildren:
            root = TreeNode(0)
            root.left = lchild
            root.right = rchild
            curList.append(root)
    return curList

def helper(n):
    if n in memo: return memo[n]
    curList = []
    for i in range((n-1)//2):
         curList += bindRoot(helper(i*2+1), helper(n-1-i*2-1))
    memo[n] = curList
    return curList

helper(n)
return memo[n]
```

#### Delete Node

+ 814 Binary Tree Pruning: remove every subtree (of the given tree) not containing a 1

```python
if not root: return None
root.left = self.pruneTree(root.left)
root.right = self.pruneTree(root.right)
if root.val: return root
return root if root.left or root.right else None
```

#### Clone

```python
copy = {} # store dict{originalNode : copyNode}
stack = [root]
while stack:
    node = stack.pop()
    copy[node] = NodeCopy(node.val)
    if node.left:
        stack.append(node.left)
    if node.right:
        stack.append(node.right)
```

#### Tree Evaluation

```python
def evaluate(self):
    if not self.left and not self.right: return self.val
    if self.val == '+': return self.left.evaluate() + self.right.evaluate()
    if self.val == '-': return self.left.evaluate() - self.right.evaluate()
    if self.val == '*': return self.left.evaluate() * self.right.evaluate()
    if self.val == '/': return self.left.evaluate() // self.right.evaluate()
```





### Morris Traversal (In Order)

```python
def inOrder(root):
    cur = root
    while cur:
        if !cur.left:
            print(cur.val)
            cur = cur.right
        else:
            pre = cur.left
            while cur.right!=cur and pre.right:
                pre = pre.right
            if !pre.right:
                pre.right = cur
                cur = cur.left
            else:
                pre.right = null
                print(cur.val)
                cur = cur.right
                
```



### BST

+ Find:

```python
def find(root, target):
    cur = root
    while cur:
        if cur.val==target: return True
        if value<target: cur=cur.left
        else: cur=cur.right
    return False


def add(root, value):
    if not root: root=TreeNode(value), return True
    cur = root
    while cur:
        if cur.val==value: return False
        if value<cur.val:
            if cur.left: cur=cur.left
            else: cur.left=TreeNode(value), return True
        else:
            if cur.right: cur = cur.right
            else: cur.right=TreeNode(value), return True
    return False

### Remove: 
#if only one child: replace cur with that child
#if two childs, use right most node in left tree, or left most node in right tree
def remove(node): #returns head after removal
    if not node.left and not node.right: return None
    if not node.left: return node.right
    if not node.right: return node.left
    node.val = findAndRemove(node)
    return node

def findAndRemove(node):
    if not node.left.right:
        result = node.left.val #copy left to cur
        node.left = node.left.left #link to left's child
        return result
    node = node.left
    while node.right.right:
        node = node.right
    result = node.right.val
    node.right = node.right.left
    return result

# remove without recursive swap:
def deleteNode(self, root: TreeNode, key: int) -> TreeNode:
    if not root: return root
    if key>root.val:
        root.right = self.deleteNode(root.right, key)
    elif key<root.val:
        root.left = self.deleteNode(root.left, key)
    else: # need to delete root
        new_root = None
        if not root.left:
            new_root = root.right
        elif not root.right:
            new_root = root.left
        else: # have two children
            parent, new_root = root, root.right
            while new_root.left: # find left most node of right subtree
                parent, new_root = new_root, new_root.left
            if new_root!=root.right: # right subtree is not leaf, move node
                parent.left, new_root.right = new_root.right, root.right
            new_root.left = root.left
        return new_root
    return root
```

+ Verify from preorder:

```python
# Use stack
if not preorder: return True
stack = []
minVal = float('-inf')
for num in preorder:
    if num<minVal: return False
    while stack and stack[-1] < num: minVal = stack.pop()
    stack.append(num)
return True
```



### Trie

```python
class TrieNode:
    def __init__():
        children = [None]*26;
        val = 0;
        isWord = False;

class Trie:
    def __init__():
        self.root = TrieNode()
    def insert(word):
        if not word: return
        cur = self.root
        for i in range(len(word)):
            c = word[i]
            idx = c - 'a'
            if not cur.children[idx]:
                newNode = TrieNode()
                cur.children[idx] = newNode
            cur = cur.children[idx]
    	cur.isWord = True
    def search(word):
        if not word: return True
        cur = self.root
        for i in range(len(word)):
            c = word[i]
            idx = c - 'a'
            if not cur.children[idx]: return False
            cur = cur.children[idx]
    	return cur.isWord        
           
```






## Graph

https://leetcode.com/discuss/general-discussion/655708/graph-for-beginners-problems-pattern-sample-solutions/

### Convert List of edges to Adjacency List

```python
graph = defaultdict(list)
for edge in edges:
    graph[edge[0]].append(edge[1])
    graph[edge[1]].append(edge[0]) # undirected
```



### DFS

+ Pattern

```python
# Recursive
def dfs(cur):
    if cur in visited: return
    visited.add(cur)
    for nei in graph[cur]:
        dfs(nei)
dfs(start)

# Iterative
stack.apend(start)
while stack:
    cur = stack.pop()
    visited.add(cur)
    for nei in graph[cur]:
        if nei not in visited:
            stack.append(nei)
```

#### Connected components  (323, 547)

+ Maintain a count label for each dfs

<img src="/home/arkyyang/files/notes/notes/attachments/image-20210213092757379.png" alt="image-20210213092757379" style="zoom:50%;" />

```python
group = [-1]*n
count = 0

for i in range(n):
    if i not in visited:
        count += 1
        dfs(i)
        
def dfs(cur):
    if cur in visited: return
    visited.add(cur)
    group[cur] = count
    for nei in graph[cur]:
        dfs(nei)
```



#### Find Cycle

+ With backtracking

```python
# 0: unvisited, 1: inPath, 2: visited

def hasCycle(i):
    if visited[i] != 0: return visited[i]==1
    visited[i] = 1
    for j in graph[i]:
        if hasCycle(j): return True
    visited[i] = 2
    return False


def notInCycle(i): #802
    if visited[i] != 0: return visited[i]==2
    visited[i] = 1
    for j in graph[i]:
        if not notInCycle(j): return False
    visited[i] = 2
    return True  


def inCycle(s,t, visited=set()):
    if s in visited: return False
    visited.add(s)
    if s==t: return True
    return any(inCycle(nei, t) for nei in graph[s])

def findCycle(i, path):
    if visited[i] != 0:
        return path if visited[i]==1 else []
    visited[i] = 1
    path.append(i)
    res = []
    for j in graph[i]:
        res = findCycle(j, path)
        if res: return res
    path.pop()
    visited[i] = 2
    return res

n = len(graph)
visited = [0]*n
for i in range(n):
    print(___Cycle(i, []))
```

+ In undirected graph

```python
        def hasCycle(v, parent):
            visited.add(v)
            for child in graph[v]:
                if child not in visited:
                    if hasCycle(child, v):
                        return True
                elif parent!= child:
                    return True
            return False
         for v in range(n):
            visited = set()
            if hasCycle(v, -1):
                return True
```

### BFS (min dist unweighed graph)

+ Min dist in graph (1129)

```python
q = deque([source])
steps = 0
seen = set()
dist = [float('inf')]*n
while q:
    size = len(q)
    steps += 1
    for i in range(size):
        cur = q.popleft()
        dist[cur] = min(dist[cur], steps)
        for nei in graph[cur]:
            if nei in seen or not isLegal(nei): continue
            seen.add(nei)
            q.append(nei)
return dist
```



+ Max/ min dist in grid: 1162, 542, 994, 1092

```python
n, m = len(grid), len(grid[0])
queue = deque((i,j,0) for i in range(n) for j in range(m) if grid[i][j] == 1)
res = -1

di = [1,-1,0,0]
dj = [0,0,1,-1]

while queue:
    i,j,d = queue.popleft()
    res = max(res, d)
    for k in range(4):
        ni,nj = i+di[k], j+dj[k]
        if 0<=ni<n and 0<=nj<m and grid[ni][nj] ==0:
            grid[ni][nj] = 1
            queue.append((ni,nj,d+1))
```



### Shortest path problems

#### Kruskal (min spanning forest undirected graph)

```python
ds = DSU(n)
heap = [(w, (u, v))...]
res = 0
while heap:
    w, (u, v) = heapq.heappop(heap)
    if ds.find(u) != ds.find(v):
        ds.union(u,v)
        res += cost
return res
```



#### Dijkstra (single source shortest weight path $O(N^2)$)

+ dist: dict of shortest distance from start

```python
def dijkstra(start, graph):
    dist = {v: float('inf') for v in N} # N = num nodes
    dist[start] = 0
    heap = [(0, start)]
    # visited = set() # only need visited if has negative weight or undirected graph
    while heap:
        curDist, cur = heapq.heappop(heap)
        # visited.add(cur)
        f(cur, curDist) # if len(visited) == N: reached all nodes
        for nei, weight in graph[cur]:
            newDist = curDist + weight
            if newDist < dist[nei]: # and nei not in visited:
                dist[nei] = newDist
                heapq.heappush(heap, (newDist, nei))
    return dist
```

#### Bellman-Ford ( single source shortest weight path $O(NE)$)

```python
dp = [[float('inf')]*n for _ in range(K+2)] # at most K steps,  K = N if not limited
dp[0][src] = 0
for i in range(1, K+2):
    dp[i][src] = 0
    for from_, to_, w in edges:
        dist[i][to_] = min(dist[i][to_], dist[i-1][from_] + w)
return dp[K+1][end] if dp[K+1][end]!=float('inf') else -1
```

#### Floyd-Warshall (All pairs $O(N^3)$, DP)

+ d: shortest distance from u to v

```python
d = [[float('inf')]*n for _ in range(n)]
for from_, to_, w in edges: d[from_][to_] = w
for from_ in range(n): d[from_][from_] = 0   
for k in range(n):
    for i in range(n):
        for j in range(n):
            d[i][j] = min(d[i][j], d[i][k] + d[k][j])
```



### Graph m- Coloring

```python
# DFS
def isSafe(v, colors, curColor):
    for nei in graph[v]:
        if colors[nei] == curColor: return False
    return True

def dfs(m, colors, v):
    if v==n: return True
    for curColor in range(1, m+1):
        if isSafe(v, colors, curColor):
            colors[v] = curColor
            if dfs(m, colors, )
# BFS

# Greedy
res = [0]*(n+1)
for cur in range(1, n+1):
    available = {1,2,3..m}
    for nei in graph[cur]:
        if res[nei] in available:
            available.remove(res[nei])
   	res[cur] = available.pop()
return res[1:]

```





### Topological Sort

+ BFS directed graph [444,207, 210]

```python
# graph processing
out_edges = defaultdict(list)
in_deg = defaultdict(int)
for f, t in edges:
    out_edges[f].append(t)
    in_deg[t] += 1
   
# topological sort
q = deque([v for v in in_deg if in_deg[v] == 0])
order = []/ count = 0
while q:
    # if len(q) != 1: not unique sorting order
    node = q.popleft()
    order.append(node) / count += 1 # if final count != num nodes: no top sort
    for nei in out_edges[node]:
        in_deg[nei] -= 1
        if not in_deg[nei]: q.append(nei)
```

+ BFS undirected graph [310: find centroid, ],

```python
# graph processing
neis = [[] for _ in range(n)]
inDeg = [0 for _ in range(n)]
for f, t in edges:
    neis[f].append(t)
    neis[t].append(f)
    inDeg[f] += 1
    inDeg[t] += 1

# top sort:
q = deque([v for v in range(n) if inDeg[v] == 1])
res = [0]
while q:
    res = list(q) # last in q will be centroid
    q2 = deque([])
    while q:
        cur = q.popleft()
        for nei in neis[cur]:
            inDeg[nei] -= 1
            if inDeg[nei] == 1: q2.append(nei) # inDeg=1 for terminal nodes
    q = q2

return res
```



+ DFS

```python
def topSort(self, graph):
    indegree = {}
    for x in graph:
        indegree[x]=0
    for i in graph:
        for j in i.neighbors:
            indegree += 1
    ans = []
    for i in graph:
        if indegree[i] == 0:
            self.dfs(i, indegree, ans)
    return ans

def dfs(self, i, indegree, ans):
    ans.append(i)
    indegree[i] -= 1
    for j in i.neighbors:
        indegree[j] -=1
        if indegree[j] ==0:
            self.dfs(j, indegree, ans)
```





### Graph Traversal

```python
visited=set()
ans
for node in nodes:
    if node in visited: continue
    dfs(node)
    f(ans)
return ans

def dfs(node):
    if node in visited: return
    visited.add(node)
    for child in node.children:
        dfs(child)
        
def dfsFindPath(s,t,res,visited):
    # -1: no path
    if s in visited: return -1
    if s==t: return res
    if not graph[s]: return -1 #s has no children
    visited.add(s)
    temp=res
    for child in graph[s]:
        if not child: temp=-1
        temp=dfsFindPath(child,t,res+[child], visited)
        if temp!=-1: break
    return temp

def bfs(node):
    queue = deque(node)
    while queue:
        cur = queue.pop()
        for child in cur.children:
            visited.add(child)
            f(child)
            queue.append(child)

def bfsFindShortestPath(s,t):
    q=[(s,0)]
    visited=set()
    while q:
        cur, d = q.pop(0)
        visited.add(cur)
        if cur==t: return d
        for child in cur.children:
            if legal:
                q.append((child, d+1))
    return -1
```

##### Example

+ 547 friend circles (adjacency matrix)

  ```python
  if not M: return 0
  n = len(M)
  visited = [False]*n
  ans = 0
  for i in range(n):
      if visited[i]: continue
      dfs(M,i,n,visited)
      ans+=1
  return ans
  
  def dfs(M,cur,n,visited):
      if visited[cur]: return
  	visited[cur] = True
      for i in range(n):
          if M[cur][i] and not visited[i]:
              dfs(M,i,n,visited)
  ```

#### Bipartite Graph

```python
color={}
for node in range(len(graph)):
    if node not in color:
        stack=[node]
        color[node]=0
        while stack:
            cur=stack.pop()
            for child in graph[cur]:
                if child not in color:
                    stack.append(child)
                    color[child]=color[cur]^1
                elif color[cur]==color[child]: 
                    return False
        return True
```



### Union Find

+ 547, 684, 947, 1319, 990, 721 ,1135

```python
class DSU: #for set size of max 1001
    def __init__(self, N):
        self.par = [i for i in range(N)]
        self.rank  = [0]*N
    def find(self, x):
        if self.par[x] != x:
            self.par[x] = self.find(self.par[x])
        return self.par[x]
    def union(self, x, y):
        xr, yr = self.find(x), self.find(y)
        if xr == yr:
            return False
        if self.rank(xr) > self.rank(yr): self.par[yr] = xr
        elif self.rank(yr) > self.rank(xr): self.par[xr] = yr
        else:
            self.par[yr] = xr
            self.rank[xr] += 1 
        return True
    
    def unionWithoutRank(self,x,y):
        xr,yr=self.find(x), self.find(y)
        if xr==yr: return False
        self.par[xr]=yr
        return True
```

+ Representing special points (source, target...): add a dummy node
+ Number of connected components: 

```python
dsu = DSU(n)
numRedundant = 0
for u,v in edgeList:
    if not dsu.union(u,v): numRedundant += 1

numConnected = len({dsu.find(i) for i in range(n)})
```

+ DSU with snapshot 1724

```python
class DSU:
    
    def __init__(self, a):
        self.par = {u:u for u in a}
        self.pool = {u:{u} for u in a}
        self.events = {u: [[-inf,u]] for u in a} #snapshot
    
    def merge(self, u, v, weight):
        rootu = self.par[u]
        rootv = self.par[v]
        
        if rootu != rootv:
            
            if len(self.pool[rootv]) > len(self.pool[rootu]): # by rank
                rootu,rootv = rootv,rootu
            
            for node in self.pool[rootv]:
                self.par[node] = rootu
                self.pool[rootu].add(node)
                self.events[node].append([weight,rootu])
```



### Articulating point

+ Critical connection 1192: DFS marking currrent steps from start, find all edges not in cycle

```python
def dfs(cur, par, parStep):
    steps[cur] = parStep + 1
    for child in g[cur]:
        if child == par: continue
        elif steps[child] == -1: # not visited
            steps[cur] = min(steps[cur], dfs(child, cur, parStep + 1))
        else: # find cycle
            steps[cur] = min(steps[cur], steps[child])

    if steps[cur] == parStep + 1 and cur != 0: res.append([par, cur]) # not in cycle
    return steps[cur]

steps = [-1]*n
res = []
dfs(0, -1, 0)
```







### Eulerian Path

Path that traverses all edges

```python
n = num vertices
m = num edges
g = adjList
in = [0]*n
out = [0]*n
path = deque([])

countInOutDegrees();
start, end = findStartEnd()
if start==-1: return
dfs(start)
if len(path) == m+1: return path
return


def countInOutDegrees():
    for v in g:
        for u in v.neighbors:
            out[v]+=1
            in[u]+=1

def findStartEnd():
    start, end = 0, 0
    hasStart, hasEnd = False
    for i in range(n):
        if (out[i]-in[i])>1 or (in[i]-out[i])>1:
            return -1,-1
        elif out[i]-in[i]==1:
            if hasStart: return -1,-1
            start = i
        elif in[i]-out[i]==1:
            if hasEnd: return -1,-1
            end = i
     return start, end

def dfs(cur):
    while (out[cur]!=0):
        out[cur] -= 1
        dfs(g[cur][-1])
    path.appendleft(cur)
```



## Bit Manipulation

+ **```count ^= (1<<val)```**: Whether count of a value is even. =0 if even, =1 if odd   [1457]
+ **```x & (x-1)```** flips least siginificant 1 to 0. ==0: has <=1 1's in x   [191, 1457]
+  



## DS Implementation

### ArrayList

+ Data: array `data[]`
+ Initialization: capacity
+ Operations: access, length, add, remove, resize. Always check bound (length, capacity)

### Stack

+ push, pop, peek $O(1)$
+ usage: recursion, DFS

### Queue

+ add (offer), remove(poll), peek (element)
+ usage: BFS, multi task queue

```java
public boolean add(String item){
    if(size==capacity){
        Exception
    } else{
        elementData[rear] = item;
        rear = (rear + 1) % capacity; //circular use of array
        size++;
        return true;
    }
}
```

+ Implement queue using 2 stacks

```java
public void push(int x){
    stack2.push(x);
}
public int pop(){
    reorder();
    return stack1.pop()
}
public int peek(){
    reorder();
    return stack1.peek();
}
public void reorder(){
    if (stack1.isEmpty){
        while (!stack2.isEmpty()){
            stack1.push(stack2.pop());
        }
    }
}
```

+ Max Stack

```java
class MaxStack{
    Stack<Integer> elements;
    Stack<Integer> maxElements;
    public MaxStack(int capacity){
        elements = new Stack<>(capacity);
        maxElements = new Stack<>(capacity);
    }
    public int size(){return elements.size()}
    public int pop(){
        maxElements.pop();
        return elements.pop();
    }
    public int max(){return maxElements.peek()}
    public void push(int value){
        int curMax;
        if (elements.size()==0){
            curMax = Integer.MIN_VALUE;
        }else{
            curMax = maxElements.peek();
        }
        elements.push(value);
        if(curMax>value){
            maxElements.push(curMax);
        }else{
            maxElements.push(value);
        }
    }
    public int peek(){
        return elements.peek();
    }
}
```

+ MaxQueue

```python
from collections import deque
class MyQueue:
    # Initialize your data structure here. 
    def __init__(self):
        self.queue = deque()
        self.maxqueue = deque()
        self.maxPos = 0
    
    # Push element x to the back of queue
    def push(self, x):
        if self.queue and x>=self.maxqueue[0]:
            while self.maxqueue:
                self.maxPos+=1
                self.maxqueue.popleft()
        self.maxqueue.append(x)
        self.queue.append(x)
        return  
    
    # Removes the element from in front of queue and returns that element. 
    def pop(self):
        if not self.queue: return -1
        if self.maxPos==0:
            self.maxqueue.popleft()
        else:
            self.maxPos-=1
        return self.queue.popleft()
    
    
    # Get the front element
    def peek(self):
        return self.queue[0]
    
    
    # Returns whether the queue is empty
    def empty(self):
        return not self.queue
        
    

    # Returns the maximal value in this queue
    def max(self):
        if not self.maxqueue:
            return -1
        return self.maxqueue[0]

def check(q):
    res = q.max()
    ans = max(list(q.queue))
    print(list(q.queue),res, res==ans)
    return res==ans
    
# q = MyQueue()
# l = [1,2,5,5,4,5]
# for i in l:
#     q.push(i)
#     check(q)
# while q.queue:
#     q.pop()
#     check(q)
```



### HashMap

+ put, get, containsKey, remove, keySet, entrySet
+ Collision:
  + Avoid: expand space (when load factor > c)
  + resolve collision: open hashing (linked list), closed hashing (next available linear scan)



+ hashCode: hash function that calculates hash value

  ```python
  def hashCode()
      if not attr: ...
      return f(attr)
  ```

  

+ equals(): check whether two objects are the same

  + no two equal keys in one hashmap
  + when equals() is overridden, hashCode should be overridden accordingly

  ```python
  def equals(self, obj):
      if self==obj: return True
      if not obj || class(obj)!=class(self): return False
      # for all attributes
      if self.attr and obj.attr and self.attr!=obj.attr: return False
      ...
      return True
  ```


### Binary Tree

```java
class TreeNode{
    int val;
    TreeNode left;
    TreeNode right;
    
    public TreeNode(int val_){
        val=val_;
        left=null;
        right=null
    }
}
```

### Graph

```java
class GraphNode{
    int val;
    List<GraphNode> neighbors;
}
```



### O(1) Data structures

+ 380 Insert (hash) Delete (hash) GetRandom (array) [hash{value-> array idx}]

![image-20210222083119790](/home/arkyyang/files/notes/notes/attachments/image-20210222083119790.png)

+ 381 Insert Delete GetRandom - Duplicates allowed [hash{value -> (indices)}] array[value]

  ![image-20210222091745212](/home/arkyyang/files/notes/notes/attachments/image-20210222091745212.png)

+ 146 LRU Cache: get (hash) + move to first (Linked-list) , put (hash) + evict last (Linked-list)

![image-20210222092133990](/home/arkyyang/files/notes/notes/attachments/image-20210222092133990.png)

```python
class LLNode:
    def __init__(self, key, val):
        self.val = val
        self.key = key
        self.prev = None
        self.next = None
        
class LList:
    def __init__(self):
        self.head = LLNode('Dummy Head', 'Dummy Head')
        self.tail = LLNode('Dummy Tail', 'Dummy Tail')
        self.head.next = self.tail
        self.tail.prev = self.head
    
    def prepend(self, node):
        node.next = self.head.next
        node.next.prev = node
        node.prev = self.head
        self.head.next = node
    
    def remove(self, node) -> LLNode:
        node.next.prev = node.prev
        node.prev.next = node.next
        return node
    
    def move_to_front(self, node):
        self.prepend(self.remove(node))
        
class LRUCache:
    def __init__(self, capacity: int):
        self.capacity = capacity
        self.cache = dict()
        self.llist = LList()
        
    def get(self, key: int) -> int:
        if key not in self.cache:
            return -1
        
        self.llist.move_to_front(self.cache[key])
        
        return self.cache[key].val

    def put(self, key: int, value: int) -> None:
        if key in self.cache: 
            self.cache[key].val = value
            self.llist.move_to_front(self.cache[key])
            return
        
        if len(self.cache) >= self.capacity:
            node = self.llist.remove(self.llist.tail.prev)
            del self.cache[node.key]
        
        newNode = LLNode(key, value)
        self.cache[key] = newNode
        self.llist.prepend(newNode)
```



+ LFU Cache: 460
  + Node: {key, value, freq, it}

<img src="/home/arkyyang/files/notes/notes/attachments/image-20210226092633202.png" alt="image-20210226092633202" style="zoom:67%;" />



## OOD

### Concepts

- Class and Object

  - Class: a blueprint for a data type, scheme
  -  Object: a specific realization of any class

- Data Encapsulation: Data Abstraction and Access Labels

  - Providing only essential information to the outside world and hiding their
    background details

  - Separate interface and implementation

  - Access Labels:  public,  private,  protected , default
    $$
    \begin{array}{|l|l|l|l|}
    \hline \text { Access } & \text { public } & \text { protected } & \text { private } \\
    \hline \text { Same class } & \text { yes } & \text { yes } & \text { yes } \\
    \hline \text { Derived classes } & \text { yes } & \text { yes } & \text { no } \\
    \hline \text { Outside classes } & \text { yes } & \text { no } & \text { no } \\
    \hline
    \end{array}
    $$

- Inheritance: base class-> derived class

- Interface/ abstract class: cannot be instantiated

  - ```java
    public interface InterfaceName{
        // with final, static fiields, abstract methods
    } 
    public abstract class AbstractName{
        // data fields, constructor, methods with implementtaion
    }
    public class Name extends AbstractName{}
    ```

- Override, overload, polymorphism

  ```java
  public abstract class Human{
     ...
     public abstract void goPee();
  }
  ```

  - Override: method in the subclass with the same signature as the one in the superclass is called

    ```java
    public class Male extends Human{
    ...
        @Override
        public void goPee(){
            System.out.println("Stand Up");
        }
    }
    ```

  - Polymorphism: create object of more general class and assign to a child instance

  ```java
  public static void main(String[] args){
      ArrayList<Human> group = new ArrayList<Human>();
      group.add(new Male());
      group.add(new Female());
      // ... add more...
  
      // tell the class to take a pee break
      for (Human person : group) person.goPee();
  }
  ```
  - **Overloading** is the action of defining multiple methods with the same name, but with different parameters. It is unrelated to either overriding or polymorphism.

### Template

1. Clarify
2. Core actors and functions (from use cases)
   1. Core actors
   2. Functions
3. Classes: 
   1. graph
   2. Explaination
4. Example Cases
   1. Flow chart
5. Code

### Example  

#### Design a parking lot

1. Clarify
   1. multiple floors, multiple entry/ exit points
   2. customers collect ticket from entry point, park, and exit at exit point
   3. customers pay via cash or credit cards
   4. system has max capacity
   5. allows multiple types of parking spots (compact, large, electric, motorcycle, handicapped)
   6. allows multiple types of vehicles
   7. per-hour fee model
2. Core actors and functions (from use cases)
   1. Core actors:
      1. Admin
      2. Customer: pay, get ticket, park
      3. parking attendant
      4. system
   2. functions
      1. Add/ remove/ edit parking floor
      2. Add/ remove/ edit parking spot
      3. Add/ remove/ edit parking attendant
      4. Take ticket
      5. Scan ticket
      6. Credit card payment
      7. Cash payment
      8. Change parking rate
3. Classes
   1. Graph: 
4. Cases
5. Code





## System Design

### Template

+ Senarios:

  + Ask: 
    + Functionalities/ Features
    + DAU: Daily/ monthly.. active users
  + Enumerate features
  + Select core features
  + Analyze
    + DAU => QPS: query per second `QPS = DAU * query per user per day / 86400 (seconds a day)`
      + Peak: usually * 3
      + Fast growing: expectation in 3 months, peak users *2
    + Read QPS (~QPS?) / Write QPS (smaller?)

+ Service:

  + Replay: go over core features, add service to each feature
  + Merge: merge similar services
  + Divide large system into micro services

+ Storage

  + Candidates:

    + Relational data base: SQL, `user Table ..`
    + NoSQL Database: 
    + File system: `Media Files`

  + Step 1: select, data structure and database type for each service

  + Step 2: Schema: 

    + Table schema: field name: field type

    + Pull vs. push

+ Optimize + maintainance

  + Scale the design
  + Highly Available (no single point of failure):
    
    + Minimum latency (no bottleneck): 
  + Scalable (caching/ sharding): 
    
  + Sharding criteria
    
  + Optimize
  
  + More features: 
  
+ Special cases: 
  
+ Maintainance
  
  + Additional considerations
  
    + Security
    + Telemetry
  
  + 
  
    

### Cheatsheet

- 1 Bit
- 1 Byte = 8 bits
- 1 Kilobyte (KB) = 1024 Bytes ≈ 1000 Bytes = 1e3
- 1 Megabyte (MB) = 1024 KB ≈ 1000 KB = 1e6
- 1 Gigabyte (GB) = 1024 MB ≈ 1000 MB = 1e9
- 1 Terabyte (TB) = 1024 GB ≈ 1000 GB = 1e12
- 1 Petabyte (PB) = 1024 TB ≈ 1000 TB = 1e15
- 1 Exabyte (EB) = 1024 PB ≈ 1000 PB = 1e18



### Details

+ Senarios:

  + Ask: 

    + Functionalities/ Features
    + DAU: Daily/ monthly.. active users

  + Enumerate features

    ```
    ○ Register / Login
    ○ User Profile Display / Edit
    ○ Upload Image / Video *
    ○ Search *
    ○ Post / Share a tweet
    ○ Timeline / News Feed
    ○ Follow / Unfollow a user
    ○ Ads *
    ```

  + Select core features

    ```
    ○ Post a Tweet
    ○ Timeline / News Feed
    ○ Follow / Unfollow a user
    ○ Register / Login
    ```

  + Analyze

    + DAU => QPS: query per second `QPS = DAU * query per user per day * 86400 (seconds a day)`
      + Peak: usually * 3
      + Fast growing: expectation in 3 months, peak users *2
    + Read QPS (~QPS?) / Write QPS (smaller?)

+ Service:

  + Replay: go over core features, add service to each feature

  + Merge: merge similar services

    ```
    UserService: Register/ Login
    Tweet Service: Post tweet, news feed
    Media Service: upload image/ video
    Frendship service: follow/ unfollow
    ```

    

  + Divide large system into micro services

  + Split/ Application/ Module

+ Storage

  + Candidates:

    + Relational data base: SQL, `user Table ..`

    + NoSQL Database: 

      ```
      Tweets;
      Social Graph;
      ```

    + File system: `Media Files`

  + Step 1: select, data base type for each service

  + Step 2: Schema: 

    + SQL: field name: field type

    $$
    \begin{array}{|l|l|}
    \hline \text { User } & \\
    \hline \text { id } & \text { integer } \\
    \hline \text { username } & \text { varchar } \\
    \hline \text { email } & \text { varchar } \\
    \hline \text { password } & \text { varchar } \\
    \hline
    \end{array}
    \begin{array}{|l|l|}
    \hline \text { Fridenship } & \\
    \hline \text { id } & \text { integer } \\
    \hline \text { from_user_id } & \text { Foreign Key } \\
    \hline \text { to_user_id } & \text { Foreign Key } \\
    \hline
    \end{array}
    \begin{array}{|l|l|}
    \hline \text { Tweet } & \\
    \hline \text { id } & \text { integer } \\
    \hline \text { user id } & \text { Foreign Key } \\
    \hline \text { content } & \text { text } \\
    \hline \text { created_at } & \text { timestamp } \\
    \hline
    \end{array}
    $$

    ​		 +  TODO: types in SQL

    + Pull vs. push (eg. news feed)
      + Pull: data only flows when requested 
        + eg. when user requests news feed, server obtain 100 tweets from each friends [N DB reads] and merge to find first 100 tweets [1 DB write]
        + High read low write
        + Pros: doesn't care if clients are online
        + When to use: abundant resource, requires real time, celebrity issue, many writes
      + Push: server push changes to user 
        + eg. server push tweet to all one's friends newsfeed field [N DB writes] when he posts a tweet. Directly read from newsfeed field when reading [1 DB read]
        + Low read high write
        + Pros: write can be async
        + When to use: opposite to pull
      + Examples:
        + Pull: facebook, twitteer
        + Push + pull: Instagram

+ Optimize + maintainance

  + Optimize
    + Issues: Cache for faster read, trade off: how many to cache
      + Pull: add to cache before DB read, cache timeline/ newsFeed
      + Push: Disk is cheap, can store lot of data in disk. 
    + More features: edit, delete, media, ads
    + Special cases: 
      + Inactive users: rank followers by weight (no need to push to or pull from inactive users)
      + celebrities: push may overload server, can use Push+pull
  + Maintainance
    + Robust
    + Scalability





Confidential information and invention assignment agreement

Personal projects

4.7 : does not relate to the business of the company.