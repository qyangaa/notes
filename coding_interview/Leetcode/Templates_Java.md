

## Tools

### Math

```java
Math.max(n1, n2) // or .min
Math.ceil(p/k) #equivalent to:
((p-1) // k) + 1 #two times faster
Math.floor(p/k) #equivalent to:
p//k
```

### max min

```java
Integer.MAX_VALUE; //or MIN_
Float.MAX_VALUE; // or MIN_
```

### Ternary operator

```java
name = ((city.getName() == null) ? "N/A" : city.getName());
```





### Array

```java
void dfs(char[][] grid); //pass as parameter
int di[] = {0, 1, 0, -1};//initialize
int sum = Arrays.stream(arr).sum();//find sum
int[] copiedArray = array.clone();//copy
Arrays.stream(tab).max().getAsInt(); // max over array
System.out.println(Arrays.toString(array));//print array
```

### String

```java
s.length(); 
s.isEmpty();
s.equals(); // '==' means pointer equals
String[] array = data.split(" ");

Character.isLetter(c) #if char is letter
Character.isLetter(s.charAt(i)) # if not parsed
s.matches("[a-zA-z]+"); # match whole string
Character.isDigit(c) #is alphanumeric

s.toLowerCase() #to lower case

# Operate on characters of string
char[] chars = s.toCharArray();
Operations...
String res = String.job(" ", chars);


String.valueOf(integer); // int -> String
int i=Integer.parseInt("200");  //String -> int

x = x.replaceAll("\\s", "");//Single white space
x = x.replaceAll("\\s+", "");//one or many white space
```

### StringBuilder

```java
StringBuilder sb = new StringBuilder();
sb.length();
sb.append(root.val);
sb.deleteCharAt(sb.length() -1);
sb.toString();
```









### List

```java
List<String> list = Arrays.asList(A); // arr to list
list.contains("A"); // if "A" in list
// check if all items in list are null
list.stream().noneMatch(Objects::nonNull);
list.stream().allMatch(x -> x == null)
```



### ArrayList

```java
List<String> l = new ArrayList<>();
l.add("");
l.remove(l.size()-1); //pop right
l.remove(0); // pop left
```



### Set

```java
Set<Integer> set = new HashSet<Integer>();
set.add();
set.remove(item);
set.contains(item);
for (Integer temp: set){}
//intersection between sets
s1.retainAll(s2) //destroy s1
Set<String> intersection = new HashSet<String>(s1); // use the copy constructor
intersection.retainAll(s2); //preserve s1
set1.addAll(set2); // Union
```





### Queue

```java
Queue<Integer> q = new LinkedList<>();
Queue<String> q = new LinkedList<String>(Arrays.asList(array)); // array -> queue
// .add(i), .peak(), .poll(), .remove(), .size()

```

### Stack

```java
Stack<TreeNode> stack = new Stack<TreeNode>();
//.push() .pop() .peek()
```





### ArrayDeque

```python
q.isEmpty();
q.getLast();
q.removeLast();
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

count = list(collections.Counter(items).items())
#sort by count (max) then alphabetical
count.sort(key=lambda x: (-x[1], x[0]))
#heap to find top k count (logk time)
heapq.nlargest(k,count, lambda x: (-x[1], x[0]))
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





### 1D, depend on $O(n)$ subproblem



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



## Search

### Binary Search

+ Time complexity: $O(log(r-1)*(f(m)+g(m)))$
+ returns left most element where g(m) holds
+ Upper bound: `m=m=l+(r-l)//2;l=m+1 `
+ Lower bound: `m=m=l+(r-l+1)//2;r=m-1`

```java
// Upper bound: right most index of target + 1
// l: smallest idx>val
// l-1: largest idx <= val

// first number > val
int l = 0;
int r = A.length;
while (l < r){
    int m = l + (r - l)/2 ;
    if (A[m] > val){
        r = m;
    }else{
        l = m + 1;
    }
}
return l;

// first number == val
int l = 0;
int r = A.length;
while (l < r){
    m = l + (r - l) / 2;
    if (A[m] >= val){
        r = m;
    }else{
        l = m + 1;
    }
}
return l;
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







### DFS & BFS

#### Grid

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
    seen=()
    queue = deque((i,j))
    while queue:
        r,c = queue.popleft()
        for d in [(r+1,c),(r-1,c),(r,c+1),(r,c-1)]:
            if illegal(d): 
                seen.append(d) #prevent reuse
                continue
            if earlyStop(d): return result
            seen.add(d) #prevent reuse
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

  

#### Combination/ Permutation

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
        queue.append([None])
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



### Iterative methods

```java

// Pre-order save space
TreeNode cur = root;
Stack<TreeNode> stack = new Stack<TreeNode>();
stack.push(root);
while(!stack.isEmpty()){
	visit(cur)
    if(cur.right!=null)
        stack.push(cur.right);
    if(cur.left!=null){
        cur = cur.left;
    }else{
        cur = stack.pop();
    }
}
```



### DFS

#### One root

```python
def f(root):
    if not root: return
    if base_case(root): return #(eg. if not root.left and not root.right)
    l = f(root.left)
    r=f(root.right)
    return g(root, l, r)
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
```




## Graph

### Convert List of edges to Adjacency List

```python
boolean[] visited = new boolean[n];
List[] graph = new List[n];
for (int i = 0; i<graph.length; i++){
    graph[i] = new ArrayList<Integer>();
}

for(int[] edge: edges){
    int u = edge[0];
    int v = edge[1];
    graph[u].add(v);
    graph[v].add(u);
}
```

### DFS

+ Template

```java
Stack<Integer> stack = new Stack<>();

for(int i = 0; i<n; i++){
    if(!visited[i]){ //over different connected components
        stack.push(i);

        while(!stack.isEmpty()){
            int cur = stack.pop();
            visited[cur] = true;
            List<Integer> neighbors = graph[cur];
            for(int nei: neighbors){
                if(!visited[nei]){
                    stack.push(nei);}}}}}
```



### Find Cycle

+ With backtracking

```python
def hasCycle(cur, graph, path, visited):
    if path[cur]: return True
    if visited[cur]: return False
    path[cur]=True
    ret = False
    for child in graph[cur]:
        ret = hasCycle(child, graph, path)
        if ret: break #not return directly because we need to backtrack
    path[cur] = False #backtrack
    visited[cur] = True
    return ret

def inCycle(s,t, visited=set()):
    if s in visited: return False
    visited.add(s)
    if s==t: return True
    return any(inCycle(nei, t) for nei in graph[s])

def findCycle(cur, path):
    if cur in path: return path
    if cur in visited: return []
    visited.add(cur)
    path.append(cur)
    ret = []
    for child in graph[cur]:
        ret = findCycle(child, path)
        if ret: return ret
        return ret
```



### Topological Sort

```python
class GNode:
    def __init__(self):
        self.inDegrees=0
        self.outNodes=[]

graph=defaultdict(GNode)
totalDeps = 0
for first, second in prerequisites:
    graph[first].outNodes.append(second)
    graph.second.inDegrees+=1
    totalDeps+=1

# find no inDegree nodes to start with
start = deque()
for index, node in graph.items():
    if node.inDegrees ==0:
        start.append(index)
        
res=[]
while start:
    cur = start.pop()
    res.append(cur)
    for child in graph[cur].outNodes:
        graph[child].inDegrees-=1
        removedEdges+=1
        if graph[child].inDegrees=0:
            start.append(child)

return res
```

### Graph

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

```python
def find(x):
    while parent[x]!=x:
        x=parent[x]
    return x

def union(x,y):
    parent[find(x)] = find(y)
    
    
def DSU: #for set size of max 1001
    def __init__(self):
        self.par=range(1001)
        self.rank=[0]*1001
        
    def find(self,x):
        if self.par!=x:
            # path compression
            self.par[x]=self.find(self.par[x])
        return self.par[x]
    
    def union(self,x,y):
        #xr, yr: rank of parent (depth of tree)
        xr,yr=self.find(x), self.find(y)
        if xr==yr: 
            return False
        elif self.rank[xr]<self.rank[yr]:
            self.par[xr]=yr
        elif self.rank[xr]>self.rank[yr]:
            self.par[yr]=xr
        else: 
            self.par[yr]=xr
            self.rank[xr]+=1
        return True
    
    def unionWithoutRank(self,x,y):
        self.par[self.find(x)]=self.find(y)
```



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

