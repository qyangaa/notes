### 81. Search in Rotated Sorted Array II

Medium

You are given an integer array `nums` sorted in ascending order (not necessarily **distinct** values), and an integer `target`.

Suppose that `nums` is rotated at some pivot unknown to you beforehand (i.e., `[0,1,2,4,4,4,5,6,6,7]` might become `[4,5,6,6,7,0,1,2,4,4]`). 

**Example 1:**

```
Input: nums = [2,5,6,0,0,1,2], target = 0
Output: true
```

**Example 2:**

```
Input: nums = [2,5,6,0,0,1,2], target = 3
Output: false
```



```python
    if not nums: return -1
    l, r = 0, len(nums)-1
    while l<r:

        m=l+(r-l)//2
        mval = nums[m]
        if mval==target:
            return True
        if mval==nums[r]: # we don't know which side it is on, so we can only move linearly
            r=r-1
            continue
        elif mval==nums[l]:
            l=l+1
            continue
        if mval<nums[r]:
            if mval<target<=nums[r]:
                l=m+1
            else:
                r=m
        else:
            if nums[l]<= target < mval:
                r = m
            else:
                l=m+1
    return l<len(nums) and nums[l]==target
```



### 179. Largest Number

Medium

Given a list of non-negative integers `nums`, arrange them such that they form the largest number.

**Note:** The result may be very large, so you need to return a string instead of an integer.

 

**Example 1:**

```
Input: nums = [10,2]
Output: "210"
```

**Example 2:**

```
Input: nums = [3,30,34,5,9]
Output: "9534330"
```

**Example 3:**

```
Input: nums = [1]
Output: "1"
```

**Example 4:**

```
Input: nums = [10]
Output: "10"
```

https://stackoverflow.com/questions/5213033/sort-a-list-of-lists-with-a-custom-compare-function

[3,3,30,30,34,5,9,89,85,78]

+ Key: comparison

```python
    def comparestr(xstr, ystr): # x larger for increasing order
        if xstr==ystr:
            return 0
        xfirst = xstr+ystr
        yfirst = ystr+xstr
        for i in range(len(xfirst)):
            if xfirst[i]>yfirst[i]: return -1
            if xfirst[i]<yfirst[i]: return 1
        return 0

    def compare(x,y):
        return comparestr(str(x),str(y))

    nums = sorted(nums,cmp=compare)
    if not nums: return "0"
    if nums[0]==0: return "0"

    return ''.join([str(n) for n in nums]) 
```

+ Slow, TODO: see how to optimize



### Search for A range: 

[lower, upper] of found number inclusive : TODO: double check

```python
def searchRange(nums, target):
    if not nums: return [-1,-1]
    l,r = 0, len(nums)-1
    found = False
    while l<r: # lower bound
        
        m=l+(r-l+1)//2
        
        if nums[m]==target:
            found = True
        if nums[m]>=target: #lowest index satisfy this
            r=m-1
        else:
            l=m
    if nums[l]!=target: l+=1
    lower = l
    
    l,r=0,len(nums)
    while l<r: # upper bound
        m=l+(r-l)//2
        if nums[m]==target:
            found = True
        if nums[m]<=target:
            l=m+1
        else:
            r=m
    upper = l
    if not found: return [-1,-1]
    return [lower+1, upper]

inputs = [[[6,5,7,7,8,8,10],7,[3,4]],
         [[6,5,7,7,8,8,10],8,[5,6]],
         [[6,5,7,8],7,[3,3]],
         [[6,5,7,8,8],8,[4,5]],
         [[6],6,[1,1]],
          [[6,6],6,[1,2]],
         [[6,5,7,8],3,[-1,-1]]]

for nums,target, sol in inputs:
    print(searchRange(nums,target), sol)
        
```





### 349. Intersection of Two Arrays

Easy

Given two arrays, write a function to compute their intersection.

**Example 1:**

```
Input: nums1 = [1,2,2,1], nums2 = [2,2]
Output: [2]
```

**Example 2:**

```
Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
Output: [9,4]
```

**Note:**

- Each element in the result must be unique.
- The result can be in any order.



+ Not subarray, just number that appear multiple times
+ create a set of one array, go over another array to find 

```python
set1 = set(nums1)
res=set()
for i in nums2:
    if i in set1: res.add(i)
return list(res)

### Built-in function:
return list(set2 & set1)
```







### 1339

![image-20210128183207393](/home/arkyyang/files/notes/notes/attachments/image-20210128183207393.png)

### 1676

129



DFS

Matrix: 



DP:

+ When result is sparse, recursion (top down) might be faster than bottom up DP.

1594



### 253. Meeting Rooms II

Medium

Given an array of meeting time intervals `intervals` where `intervals[i] = [starti, endi]`, return *the minimum number of conference rooms required*.

 

**Example 1:**

```
Input: intervals = [[0,30],[5,10],[15,20]]
Output: 2
```

**Example 2:**

```
Input: intervals = [[7,10],[2,4]]
Output: 1
```

 

+ sort by start time
  + check max number of overlap at any given time
  + if min(prevEnd) < curStart: prevEnd.pop(min)
  + ex: `[0,20], [5,25],[20, 27]`

```python
    intervals.sort(key=lambda x: x[0])
    prevEnd = []
    maxRooms = 1
    for item in intervals:
        while prevEnd and prevEnd[0]<=item[0]:
            heapq.heappop(prevEnd)
        heapq.heappush(prevEnd, item[1])
        maxRooms = max(maxRooms, len(prevEnd))

    return maxRooms
```





### Raindrops

**时间限制：** 3000MS
**内存限制：** 589824KB

**题目描述：**

When it rains, each raindrop could cover a certain area. If we have an array of each raindrop, could you tell k that the first k raindrops can final make this sidewalk all wet? The input will be a 2-dimenstion array. Sidewalk will be [0,1] always N lines with 2 numbers in each line, meaning the range that a raindrop could cover. Return k if the first k raindrop could already cover the sidewalk. Return -1 if all the raindrops can still not cover the sidewalk. 



样例输入

```
example 1: [0, 0.3] [0.3, 0.6] [0.5, 0.8] [0.67, 1] [0, 0.4] [0.5, 0.75] 

example 2: [0, 0.3] [0.52, 0.8] [0.76, 1] [0.6, 0.98]
```

样例输出

```
example 1: 4 since the first 4 raindrops  can cover the sidewalk
example 2: -1
```

+ Merge intervals

```python
def rainDrops(rain):
    def helper(rain):
        rain.sort()
        curEnd = 0
        for drop in rain:
            if curEnd<drop[0]:
                return False
            curEnd = max(curEnd, drop[1])
        return curEnd>=1
    for i in range(len(rain)):
        if helper(rain[:i+1]):
            return i+1
    return -1
```

+ solution

```java
    static int rainDrops(double[][] rain) {
        ArrayList<Interval> intervals = new ArrayList<>();
        intervals.add(new Interval(rain[0][0], rain[0][1]));
        for (int i = 1; i < rain.length; i ++) {
            if (intervals.size() == 1 && intervals.get(0).start < 1e-5 && intervals.get(0).end >= 1 - 1e-5) {
                return i;
            }
            insert(intervals, new Interval(rain[i][0], rain[i][1]));
        }
        if (intervals.size() == 1 && intervals.get(0).start < 1e-5 && intervals.get(0).end >= 1 - 1e-5) {
            return rain.length;
        } else {
            return -1;
        }
    }

    static void insert(List<Interval> intervals, Interval newInterval) {
        ListIterator<Interval> it = intervals.listIterator();  
        while(it.hasNext()) {  
            Interval tmpInterval = it.next();  
            if(newInterval.end < tmpInterval.start) {  
                it.previous();  
                it.add(newInterval);  
                return;  
            } else {  
                if(tmpInterval.end < newInterval.start) {  
                    continue;  
                } else {  
                    newInterval.start = Math.min(tmpInterval.start, newInterval.start);  
                    newInterval.end   = Math.max(tmpInterval.end, newInterval.end);  
                    it.remove();  
                }  
            }  
        }
        intervals.add(newInterval);  
    }
    static class Interval{
        double start;
        double end;
        Interval(double s, double e) {
            start = s;
            end = e;
        }
    }
```



### M coloring

Given an undirected graph and a number m, determine if the graph can be colored with at most m colors such that no two adjacent vertices of the graph are colored with same color. Here coloring of a graph means assignment of colors to all vertices.

```python

def graphColoring(graph, m):
    
    def dfs(cur):
        for c in range(m):
            inChildren = False
            for v in adjList[c]:
                if color[v] == c:
                    inChildren = True
                    break
            if inChildren: continue
            color[cur] = c
            visited.add(cur)
            curColorFound = True
            for v in adjList[c]:
                if v not in visited:
                    curColorFound = curColorFound and dfs(v)
            if len(visited)==len(graph) and curColorFound:
                return True
            color[cur] = -1
        return False
    
    
    n = len(graph)
    adjList = [[]*n for _ in range(n)]
    for u, row in enumerate(graph):
        for v, item in enumerate(row):
            if item:
                adjList[u].append(v)
                
    color = [-1]*n
    visited = set()
    for v in range(n):
        if not dfs(v):
            return False
    return True

    
    

tests = [[[
        [ 0, 1, 1, 1 ],
        [ 1, 0, 1, 0 ],
        [ 1, 1, 0, 1 ],
        [ 1, 0, 1, 0 ],
    ], 3],[[
        [ 0, 1, 1, 1 ],
        [ 1, 0, 1, 1 ],
        [ 1, 1, 0, 1 ],
        [ 1, 1, 1, 0 ],
    ], 3],[[
        [ 1, 1, 1, 1 ],
        [ 1, 1, 1, 1 ],
        [ 1, 1, 1, 1 ],
        [ 1, 1, 1, 1 ],
    ], 3],[[
        [ 0, 1, 1 ],
        [ 1, 0, 1 ],
        [ 1, 1, 0 ],
    ], 2], [[[0,1],[1,0]],2],[[[0,1],[1,0]],0],[[],2],[[[0]],1],[[
        [ 0, 1, 1 ],
        [ 1, 0, 1 ],
        [ 1, 1, 0 ],
    ], 3] ]

for test in tests:
    print(graphColoring(test[0], test[1]))
```



+ Solution

```java
    static boolean graphColoring(int[][] graph, int m) {
        int[] color = new int[graph.length];
        return graphColoringUtil(graph, m, color, 0);
    }

    static boolean graphColoringUtil(int graph[][], int m,
                              int[] color, int v)
    {
        if (v == graph.length)
            return true;

        for (int c = 1; c <= m; c++)
        {
            if (isSafe(v, graph, color, c))
            {
                color[v] = c;
 
                if (graphColoringUtil(graph, m, color, v + 1))
                    return true;
                color[v] = 0;
            }
        }
        return false;
    }   

    static boolean isSafe(int v, int graph[][], int color[],
                   int c)
    {
        for (int i = 0; i < graph.length; i++)
            if (graph[v][i] == 1 && c == color[i])
                return false;
        return true;
    }    
```





### Minimum deletion

Given a string and a dictionary, calculate how many characters need to be removed at least, so that the document is composed by the words in the dictionary.



输入描述

a string and a string array  

输出描述

minimum number of characters you need to remove. If no available word can be formed, return the length of the string since all the characters have to be removed.



样例输入

```
hellowworld
2
hello
world
```

样例输出

```
1
```





```python
def  removeCharacters(str, dict):
    d = set(dict)
    n = len(str)
    if not d or n==0: return n
    dp = [[n]*n for _ in range(n)]
    for l in range(1,n+1):
        for i in range(n-l+1):
            j = i+l-1
            dp[i][j] = j-i+1
            if str[i:j+1] in d:
                dp[i][j] = 0
                continue
            for k in range(i,j+1):
                if str[k:j+1] in d:
                    dp[i][j] = min(dp[i][j], dp[i][k-1])
            dp[i][j] = min(dp[i][j], dp[i][j-1]+1)
    return dp[0][n-1]

tests = [['',['a']],['baaabaa',['a','aaa']], ['abcdabo',['ab','abc','bo']], ['abcdabo',['ab','abc', 'bo', 'da']],['hellowworld', ['hello','world']],['hemllowwormld', ['hello','world']]]

for test in tests:
    print(removeCharacters(test[0], test[1]))
```



+ Solution

```java
    static int calcMaxMatch(String str, String[] dict){
        int maxMatchLen = 0;
        for(String word:dict){
            int m = 0,i =0, j =0;
            do{
                if (str.charAt(j) == word.charAt(i)) {
                    i++;
                    if (i == word.length()) { m = word.length();break;}
                }
                j++;
            }while(j < str.length());
            if(m > maxMatchLen){
                maxMatchLen =m;
            }
        }
        return maxMatchLen;
    }
    static int removeCharacters(String str, String[] dict)
    {
        int len = str.length();
        int[] dp= new int[len + 1];
        dp[0] = 0;
        for(int i = 1; i <= len ; i++) dp[i] = Integer.MAX_VALUE;
        for(int i =1 ; i <= len ; i++){
            for(int j = 0 ; j < i ; j++){
                String substr = str.substring(j,i);
                int maxMatch = calcMaxMatch(substr, dict);
                if (dp[j]!=Integer.MAX_VALUE && dp[i] > dp[j] + i - j  - maxMatch) {
                    dp[i] = dp[j] + i - j  - maxMatch;
                }
            }
        }
        return dp[len];
    }
```

