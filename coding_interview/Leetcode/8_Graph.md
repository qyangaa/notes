### 133. Clone Graph

Medium

Given a reference of a node in a **[connected](https://en.wikipedia.org/wiki/Connectivity_(graph_theory)#Connected_graph)** undirected graph.

Return a [**deep copy**](https://en.wikipedia.org/wiki/Object_copying#Deep_copy) (clone) of the graph.

Each node in the graph contains a val (`int`) and a list (`List[Node]`) of its neighbors.

```
class Node {
    public int val;
    public List<Node> neighbors;
}
```

 

**Test case format:**

For simplicity sake, each node's value is the same as the node's index (1-indexed). For example, the first node with `val = 1`, the second node with `val = 2`, and so on. The graph is represented in the test case using an adjacency list.

**Adjacency list** is a collection of unordered **lists** used to represent a finite graph. Each list describes the set of neighbors of a node in the graph.

The given node will always be the first node with `val = 1`. You must return the **copy of the given node** as a reference to the cloned graph.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2019/11/04/133_clone_graph_question.png)

```
Input: adjList = [[2,4],[1,3],[2,4],[1,3]]
Output: [[2,4],[1,3],[2,4],[1,3]]
Explanation: There are 4 nodes in the graph.
1st node (val = 1)'s neighbors are 2nd node (val = 2) and 4th node (val = 4).
2nd node (val = 2)'s neighbors are 1st node (val = 1) and 3rd node (val = 3).
3rd node (val = 3)'s neighbors are 2nd node (val = 2) and 4th node (val = 4).
4th node (val = 4)'s neighbors are 1st node (val = 1) and 3rd node (val = 3).
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/01/07/graph.png)

```
Input: adjList = [[]]
Output: [[]]
Explanation: Note that the input contains one empty list. The graph consists of only one node with val = 1 and it does not have any neighbors.
```

**Example 3:**

```
Input: adjList = []
Output: []
Explanation: This an empty graph, it does not have any nodes.
```

**Example 4:**

![img](https://assets.leetcode.com/uploads/2020/01/07/graph-1.png)

```
Input: adjList = [[2],[1]]
Output: [[2],[1]]
```

+ Two heads, one for cloned head. Then start from these heads, do recursion DFS
+ Non-directed graph, pointers can go back to earlier node, need to store a seen() list

```python
class Solution:
    def cloneGraph(self, node: 'Node',visited=defaultdict()) -> 'Node':
        if not node:
            return node
        if node in visited:
            return visited[node]
        sourceHead = node
        targetHead = Node(node.val)
        targetNeighbors=[]
        visited[node]=targetHead
        for child in sourceHead.neighbors:
                targetNeighbors.append(self.cloneGraph(child,visited))
        targetHead.neighbors = targetNeighbors
        return targetHead
```



### 841. Keys and Rooms

Medium

There are `N` rooms and you start in room `0`. Each room has a distinct number in `0, 1, 2, ..., N-1`, and each room may have some keys to access the next room. 

Formally, each room `i` has a list of keys `rooms[i]`, and each key `rooms[i][j]` is an integer in `[0, 1, ..., N-1]` where `N = rooms.length`. A key `rooms[i][j] = v` opens the room with number `v`.

Initially, all the rooms start locked (except for room `0`). 

You can walk back and forth between rooms freely.

Return `true` if and only if you can enter every room.



**Example 1:**

```
Input: [[1],[2],[3],[]]
Output: true
Explanation:  
We start in room 0, and pick up key 1.
We then go to room 1, and pick up key 2.
We then go to room 2, and pick up key 3.
We then go to room 3.  Since we were able to go to every room, we return true.
```

**Example 2:**

```

Input: [[1,3],[3,0,1],[2],[0]]
Output: false
Explanation: We can't enter the room with number 2.
```

+ The graph is given in adjacency list, start from room 0
+ BFS, mark visited on the go

```python
from collections import deque

class Solution:
    def canVisitAllRooms(self, rooms: List[List[int]]) -> bool:
        if len(rooms)<=1 : return True
        visited=set()
        queue= deque([0])
        while queue:
            cur = queue.popleft()
            visited.add(cur)
            for key in rooms[cur]:
                if key not in visited:
                    queue.append(key)
        return len(visited)==len(rooms)
```



### 207. Course Schedule

Medium

There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses-1`.

Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: `[0,1]`

Given the total number of courses and a list of prerequisite **pairs**, is it possible for you to finish all courses?

 

**Example 1:**

```
Input: numCourses = 2, prerequisites = [[1,0]]
Output: true
Explanation: There are a total of 2 courses to take. 
             To take course 1 you should have finished course 0. So it is possible.
```

**Example 2:**

```
Input: numCourses = 2, prerequisites = [[1,0],[0,1]]
Output: false
Explanation: There are a total of 2 courses to take. 
             To take course 1 you should have finished course 0, and to take course 0 you should
             also have finished course 1. So it is impossible.
```

+ Topological sort: whether exists a topological ordering. Good as long as there is no cycle in the graph. Each pair `[0,1]` gives an edge from left to right. First construct an edge list
+ Keep a visited graph. If child in in visited graph, then is cyclic, return False. 

+ Solution: DFS finding cycles $O(|E|+|V|^2)$. If prevent checking nodes repetitively, can get to $O(|E|+|V|)$

```python
edgeList = defaultdict()
for edge in prerequisites:
    edgeList[edge[0]]=edge[1]

path = [False]*numCourses
visited=[False]*numCourses
for cur in range(numCourses):
    if hasCycle(cur, edgeList, visited, path): return False
return True
    
def hasCycle(cur, edgeList, visited, path):
    if visited[cur]: return False
    if path[cur]: return True
    path[cur]=True
    ret = False
    for child in edgeList[cur]:
        ret = hasCycle(child, edgeList, path)
        if ret: break #not return directly because we need to backtrack
    path[cur] = False #backtrack
    visited[cur] = True
    return ret


```

+ Topological sort

``` python
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
        
removedEdges = 0
while start:
    cur = start.pop()
    for child in graph[cur].outNodes:
        graph[child].inDegrees-=1
        removedEdges+=1
        if graph[child].inDegrees=0:
            start.append(child)
if removedEdges == totalDeps:
    return True
else:
    return False
```



### 399. Evaluate Division

Medium

You are given an array of variable pairs `equations` and an array of real numbers `values`, where `equations[i] = [Ai, Bi]` and `values[i]` represent the equation `Ai / Bi = values[i]`. Each `Ai` or `Bi` is a string that represents a single variable.

You are also given some `queries`, where `queries[j] = [Cj, Dj]` represents the `jth` query where you must find the answer for `Cj / Dj = ?`.

Return *the answers to all queries*. If a single answer cannot be determined, return `-1.0`.

**Note:** The input is always valid. You may assume that evaluating the queries will not result in division by zero and that there is no contradiction.

 

**Example 1:**

```
Input: equations = [["a","b"],["b","c"]], values = [2.0,3.0], queries = [["a","c"],["b","a"],["a","e"],["a","a"],["x","x"]]
Output: [6.00000,0.50000,-1.00000,1.00000,-1.00000]
Explanation: 
Given: a / b = 2.0, b / c = 3.0
queries are: a / c = ?, b / a = ?, a / e = ?, a / a = ?, x / x = ?
return: [6.0, 0.5, -1.0, 1.0, -1.0 ]
```

**Example 2:**

```
Input: equations = [["a","b"],["b","c"],["bc","cd"]], values = [1.5,2.5,5.0], queries = [["a","c"],["c","b"],["bc","cd"],["cd","bc"]]
Output: [3.75000,0.40000,5.00000,0.20000]
```

**Example 3:**

```
Input: equations = [["a","b"]], values = [0.5], queries = [["a","b"],["b","a"],["a","c"],["x","y"]]
Output: [0.50000,2.00000,-1.00000,-1.00000]
```

+ Each query [s->t] is equal to mutiplications of all intermediate edges
+ Construct adjacency list as:{s:[(t, val)...]}, equation item gives two edges: one with val, one with `1/val`
+ Real numbers: no negative value exist



### 785. Is Graph Bipartite?

Medium

Given an undirected `graph`, return `true` if and only if it is bipartite.

Recall that a graph is *bipartite* if we can split its set of nodes into two independent subsets A and B, such that every edge in the graph has one node in A and another node in B.

The graph is given in the following form: `graph[i]` is a list of indexes `j` for which the edge between nodes `i` and `j` exists. Each node is an integer between `0` and `graph.length - 1`. There are no self edges or parallel edges: `graph[i]` does not contain `i`, and it doesn't contain any element twice.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/10/21/bi1.jpg)

```
Input: graph = [[1,3],[0,2],[1,3],[0,2]]
Output: true
Explanation: We can divide the vertices into two groups: {0, 2} and {1, 3}.
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/10/21/bi2.jpg)

```
Input: graph = [[1,2,3],[0,2],[0,1,3],[0,2]]
Output: false
Explanation: We cannot find a way to divide the set of nodes into two independent subsets.
```

+ Graph is given in adjacency list with edge: idx-> items
+ Total number of vertices is len(graph)
+ We need to go over all edges, thus BFS and mark every edge as either 0 or 1 in altering order. 

```python
from collections import deque
class Solution:
    def isBipartite(self, graph: List[List[int]]) -> bool:
        if len(graph)<=1: return True
        mark=[-1 for _ in range(len(graph))]
        mark[0]=0
        queue=deque([0])
        tovisit=set([i for i in range(len(graph))])
        while queue:
            cur = queue.popleft()
            tovisit.remove(cur)
            for child in graph[cur]:
                legalMark = 1-mark[cur]
                if mark[child]==-1: 
                    mark[child]=legalMark
                    queue.append(child)
                elif mark[child]!=legalMark:
                    return False
            if (not queue) and tovisit:
                temp = tovisit.pop()
                queue.append(temp)
                mark[temp]=0
                tovisit.add(temp)
        return True
```

solution: use dictionary and DFS. Faster

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



### 997. Find the Town Judge

Easy

In a town, there are `N` people labelled from `1` to `N`. There is a rumor that one of these people is secretly the town judge.

If the town judge exists, then:

1. The town judge trusts nobody.
2. Everybody (except for the town judge) trusts the town judge.
3. There is exactly one person that satisfies properties 1 and 2.

You are given `trust`, an array of pairs `trust[i] = [a, b]` representing that the person labelled `a` trusts the person labelled `b`.

If the town judge exists and can be identified, return the label of the town judge. Otherwise, return `-1`.

 

**Example 1:**

```
Input: N = 2, trust = [[1,2]]
Output: 2
```

**Example 2:**

```
Input: N = 3, trust = [[1,3],[2,3]]
Output: 3
```

**Example 3:**

```
Input: N = 3, trust = [[1,3],[2,3],[3,1]]
Output: -1
```

**Example 4:**

```
Input: N = 3, trust = [[1,2],[2,3]]
Output: -1
```

**Example 5:**

```
Input: N = 4, trust = [[1,3],[1,4],[2,3],[2,4],[4,3]]
Output: 3
```

+ Graph is given in list of edges, convert to adjacency list.
+ Find node with no out degree but all other nodes goes in
+ When construct adjacency list, keep two numbers:
  + outDegree = len(list)
  + inDegeree
+ We only need to keep these two numbers. No need to construct an adjacency list because there is no propagation. Two lists: one for out degree, one for in degree

```python
class Solution:
    def findJudge(self, N: int, trust: List[List[int]]) -> int:
        outDegree = [0]*N
        inDegree=[0]*N
        for s,t in trust:
            outDegree[s-1]+=1
            inDegree[t-1]+=1
        for i in range(N):
            if outDegree[i]==0 and inDegree[i]==N-1: return i+1
        return -1
```



### 433. Minimum Genetic Mutation

Medium

A gene string can be represented by an 8-character long string, with choices from `"A"`, `"C"`, `"G"`, `"T"`.

Suppose we need to investigate about a mutation (mutation from "start" to "end"), where ONE mutation is defined as ONE single character changed in the gene string.

For example, `"AACCGGTT"` -> `"AACCGGTA"` is 1 mutation.

Also, there is a given gene "bank", which records all the valid gene mutations. A gene must be in the bank to make it a valid gene string.

Now, given 3 things - start, end, bank, your task is to determine what is the minimum number of mutations needed to mutate from "start" to "end". If there is no such a mutation, return -1.

**Note:**

1. Starting point is assumed to be valid, so it might not be included in the bank.
2. If multiple mutations are needed, all mutations during in the sequence must be valid.
3. You may assume start and end string is not the same.

 

**Example 1:**

```
start: "AACCGGTT"
end:   "AACCGGTA"
bank: ["AACCGGTA"]

return: 1
```

 

**Example 2:**

```
start: "AACCGGTT"
end:   "AAACGGTA"
bank: ["AACCGGTA", "AACCGCTA", "AAACGGTA"]

return: 2
```

 

**Example 3:**

```
start: "AAAAACCC"
end:   "AACCCCCC"
bank: ["AAAACCCC", "AAACCCCC", "AACCCCCC"]

return: 3
```



+ Edge exist: item differ by one char
+ Unidirectional graph, find shortest path: BFS. If no path: return -1

```python
from collections import deque
class Solution:
    def minMutation(self, start: str, end: str, bank: List[str]) -> int:
              
        
        def constructGraph(words):
            n = len(words)
            graph=defaultdict(list)
            for i in range(n):
                for j in range(i+1,n):
                    if differByOne(words[i], words[j]):
                        graph[words[i]].append(words[j])
                        graph[words[j]].append(words[i])
            return graph
                    
                    
        def differByOne(word1, word2):
            dif=0
            for i in range(8):
                if word1[i]!=word2[i]:
                    dif+=1
                if dif>1: return False
            return True
        
        graph = constructGraph(bank+[start])
        if end not in graph: return -1
        
        print(graph)
        visited = set()
        queue=deque([(start,0)])
        while queue:
            cur = queue.popleft()
            visited.add(cur)
            for child in graph[cur[0]]:
                if child==end: return cur[1]+1
                if child not in visited:
                    queue.append((child,cur[1]+1))
        return -1
```

+ Can skip graph construction and find dict while we go to save space and time

```python
def adj(s,t):
    count=0
    for i,j in zip(s,t):
        if i!=j: count+=1
    return count==1

q=[(start,0)]
visited=set()

while q:
    cur, cnt = q.pop(0)
    visited.add(cur)
    if cur==end: return cnt
    for word in bank:
        if adj(cur,word) and word not in visited:
            q.append((word, cnt+1))
return -1
```



### 684. Redundant Connection

Medium

In this problem, a tree is an **undirected** graph that is connected and has no cycles.

The given input is a graph that started as a tree with N nodes (with distinct values 1, 2, ..., N), with one additional edge added. The added edge has two different vertices chosen from 1 to N, and was not an edge that already existed.

The resulting graph is given as a 2D-array of `edges`. Each element of `edges` is a pair `[u, v]` with `u < v`, that represents an **undirected** edge connecting nodes `u` and `v`.

Return an edge that can be removed so that the resulting graph is a tree of N nodes. If there are multiple answers, return the answer that occurs last in the given 2D-array. The answer edge `[u, v]` should be in the same format, with `u < v`.

**Example 1:**

```
Input: [[1,2], [1,3], [2,3]]
Output: [2,3]
Explanation: The given undirected graph will be like this:
  1
 / \
2 - 3
```



**Example 2:**

```
Input: [[1,2], [2,3], [3,4], [1,4], [1,5]]
Output: [1,4]
Explanation: The given undirected graph will be like this:
5 - 1 - 2
    |   |
    4 - 3
```



**Note:**

The size of the input 2D-array will be between 3 and 1000.

Every integer represented in the 2D-array will be between 1 and N, where N is the size of the input array.



+ The added edge must give a cycle. Task is to find the cycle and return the edge that appears last in the input
+ Solution: DFS:

```python
class Solution(object):
    def findRedundantConnection(self, edges):
        graph = collections.defaultdict(set)

        def hasCycle(s,t):
            if s in visited: return False
            visited.add(s)
            if s==t: return True
            return any(hasCycle(child,t) for child in graph[s])
        
        for u,v in edges:
            visited = set()
            if u in graph and v in graph and hasCycle(u,v): return [u,v]
            graph[u].add(v)
            graph[v].add(u)
```



+ Solution: Union find. Cycle occurs when dsu can no longer union

```python
class DSU(object):
    def __init__(self):
        self.par = range(1001)
        self.rnk = [0] * 1001

    def find(self, x):
        if self.par[x] != x:
            self.par[x] = self.find(self.par[x])
        return self.par[x]

    def union(self, x, y):
        xr, yr = self.find(x), self.find(y)
        if xr == yr:
            return False
        elif self.rnk[xr] < self.rnk[yr]:
            self.par[xr] = yr
        elif self.rnk[xr] > self.rnk[yr]:
            self.par[yr] = xr
        else:
            self.par[yr] = xr
            self.rnk[xr] += 1
        return True
    
class DSU(object):
    def __init__(self):
        self.par = [i for i in range(1001)]
    def find(self,x):
        if self.par[x] != x:
            self.par[x] = self.find(self.par[x])
        return self.par[x]
    def union(self, x,y):
        xp, yp = self.find(x), self.find(y)
        if xp==yp: return False
        self.par[xp] = yp
        return True

dsu=DSU()
for edge in edges:
    if not dsu.union(*edge):
        return edge
```



### 261. Graph Valid Tree

Medium

Given `n` nodes labeled from `0` to `n-1` and a list of undirected edges (each edge is a pair of nodes), write a function to check whether these edges make up a valid tree.

**Example 1:**

```
Input: n = 5, and edges = [[0,1], [0,2], [0,3], [1,4]]
Output: true
```

**Example 2:**

```
Input: n = 5, and edges = [[0,1], [1,2], [2,3], [1,3], [1,4]]
Output: false
```

**Note**: you can assume that no duplicate edges will appear in `edges`. Since all edges are undirected, `[0,1]` is the same as `[1,0]` and thus will not appear together in `edges`.



+ two requirements:
  + len(edges) = n-1
  + no cycles



```python
from collections import defaultdict
class Solution:
    def validTree(self, n: int, edges: List[List[int]]) -> bool:
        if len(edges) != n-1: return False
        
        def hasCycle(v, parent):
            visited.add(v)
            for child in graph[v]:
                if child not in visited:
                    if hasCycle(child, v):
                        return True
                elif parent!= child:
                    return True
            return False
        
        
        graph = defaultdict(list)
        
        for edge in edges:
            graph[edge[0]].append(edge[1])
            graph[edge[1]].append(edge[0])
        
        for v in range(n):
            visited = set()
            if hasCycle(v, -1):
                return False
        return True
        
```





### 269. Alien Dictionary

Hard

There is a new alien language that uses the English alphabet. However, the order among letters are unknown to you.

You are given a list of strings `words` from the dictionary, where `words` are **sorted lexicographically** by the rules of this new language.

*Derive the order of letters in this language, and return it*. If the given input is invalid, return `""`. If there are multiple valid solutions, return **any of them**.

 

**Example 1:**

```
Input: words = ["wrt","wrf","er","ett","rftt"]
t->f
w->e
r->t
e->r
Output: "wertf"
```

**Example 2:**

```
Input: words = ["z","x"]
Output: "zx"
```

**Example 3:**

```
Input: words = ["z","x","z"]
Output: ""
Explanation: The order is invalid, so return "".
```

+ Topological sort

```python
from collections import defaultdict
class Solution:
    def alienOrder(self, words: List[str]) -> str:
        edges = defaultdict(list)
        indegree = defaultdict(int)
        chars = set()
        
        for w in words:
            for c in w:
                chars.add(c)
        
        n = len(chars)
        
        for i in range(len(words)-1):
            first, second = words[i], words[i+1]
            j=0
            while j<len(first) and j<len(second):
                if first[j]==second[j]:
                    j+=1
                else:
                    edges[first[j]].append(second[j])
                    indegree[second[j]]+=1
                    break
            if j==len(second) and len(first)>len(second): return ""
                
        res = []  
        
        
        def removeEdge(v):
            if indegree[v]<=0 and v in chars:
                res.append(v)
                chars.remove(v)
                for child in edges[v]:
                    indegree[child]-=1
                    removeEdge(child)
            return
                
                    
        
        for v in list(chars):
            if not indegree[v]:
                removeEdge(v)       
        
        if len(res)!=n: return ""
        
        return "".join(res)
```



### 332. Reconstruct Itinerary

Medium

Given a list of airline tickets represented by pairs of departure and arrival airports `[from, to]`, reconstruct the itinerary in order. All of the tickets belong to a man who departs from `JFK`. Thus, the itinerary must begin with `JFK`.

**Note:**

1. If there are multiple valid itineraries, you should return the itinerary that has the **smallest lexical order** when read as a single string. For example, the itinerary `["JFK", "LGA"]` has a smaller lexical order than `["JFK", "LGB"]`.
2. All airports are represented by three capital letters (IATA code).
3. You may assume all tickets form at least one valid itinerary.
4. One must use all the tickets once and only once.

**Example 1:**

```
Input: [["MUC", "LHR"], ["JFK", "MUC"], ["SFO", "SJC"], ["LHR", "SFO"]]
Output: ["JFK", "MUC", "LHR", "SFO", "SJC"]
```

**Example 2:**

```
Input: [["JFK","SFO"],["JFK","ATL"],["SFO","ATL"],["ATL","JFK"],["ATL","SFO"]]
Output: ["JFK","ATL","JFK","SFO","ATL","SFO"]
Explanation: Another possible reconstruction is ["JFK","SFO","ATL","JFK","ATL","SFO"].
             But it is larger in lexical order.
```









### 210. Course Schedule II

Medium

There are a total of `n` courses you have to take labelled from `0` to `n - 1`.

Some courses may have `prerequisites`, for example, if `prerequisites[i] = [ai, bi]` this means you must take the course `bi` before the course `ai`. `b->a`

Given the total number of courses `numCourses` and a list of the `prerequisite` pairs, return the ordering of courses you should take to finish all courses.

If there are many valid answers, return **any** of them. If it is impossible to finish all courses, return **an empty array**.

 

**Example 1:**

```
Input: numCourses = 2, prerequisites = [[1,0]]
Output: [0,1]
Explanation: There are a total of 2 courses to take. To take course 1 you should have finished course 0. So the correct course order is [0,1].
```

**Example 2:**

```
Input: numCourses = 4, prerequisites = [[1,0],[2,0],[3,1],[3,2]]
Output: [0,2,1,3]
Explanation: There are a total of 4 courses to take. To take course 3 you should have finished both courses 1 and 2. Both courses 1 and 2 should be taken after you finished course 0.
So one correct course order is [0,1,2,3]. Another correct ordering is [0,2,1,3].
```

**Example 3:**

```
Input: numCourses = 1, prerequisites = []
Output: [0]
```



```python
 edges = defaultdict(list)
        indegree = defaultdict(int)
        tovisit = set([i for i in range(numCourses)])
        for a,b in prerequisites:
            edges[b].append(a)
            indegree[a]+=1
        
        def remove(cur):
            if indegree[cur]==0 and cur in tovisit:
                res.append(cur)
                tovisit.remove(cur)
                for child in edges[cur]:
                    indegree[child]-=1
                    remove(child)
            return
        
        res = []
        for v in list(tovisit):
            if not indegree[v]:
                remove(v)
        
        if len(res) != numCourses: return ""
        
        return res
```





### 797. All Paths From Source to Target

Medium

Given a directed acyclic graph (**DAG**) of `n` nodes labeled from 0 to n - 1, find all possible paths from node `0` to node `n - 1`, and return them in any order.

The graph is given as follows: `graph[i]` is a list of all nodes you can visit from node `i` (i.e., there is a directed edge from node `i` to node `graph[i][j]`).

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/09/28/all_1.jpg)

```
Input: graph = [[1,2],[3],[3],[]]
Output: [[0,1,3],[0,2,3]]
Explanation: There are two paths: 0 -> 1 -> 3 and 0 -> 2 -> 3.
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/09/28/all_2.jpg)

```
Input: graph = [[4,3,1],[3,2,4],[3],[4],[]]
Output: [[0,4],[0,3,4],[0,1,3,4],[0,1,2,3,4],[0,1,4]]
```

**Example 3:**

```
Input: graph = [[1],[]]
Output: [[0,1]]
```

**Example 4:**

```
Input: graph = [[1,2,3],[2],[3],[]]
Output: [[0,1,2,3],[0,2,3],[0,3]]
```

**Example 5:**

```
Input: graph = [[1,3],[2],[3],[]]
Output: [[0,1,2,3],[0,3]]
```

 

+ dfs

```python
        def dfs(cur, path):
            if cur==len(graph)-1:
                res.append(list(path)+[cur])
                return
            for child in graph[cur]:
                path.append(cur)
                dfs(child, path)
                path.pop()
            return
        
        res = []
        dfs(0, [])
        return res
```





### 685. Redundant Connection II

Hard

In this problem, a rooted tree is a **directed** graph such that, there is exactly one node (the root) for which all other nodes are descendants of this node, plus every node has exactly one parent, except for the root node which has no parents.

The given input is a directed graph that started as a rooted tree with `n` nodes (with distinct values from `1` to `n`), with one additional directed edge added. The added edge has two different vertices chosen from `1` to `n`, and was not an edge that already existed.

The resulting graph is given as a 2D-array of `edges`. Each element of `edges` is a pair `[ui, vi]` that represents a **directed** edge connecting nodes `ui` and `vi`, where `ui` is a parent of child `vi`.

Return *an edge that can be removed so that the resulting graph is a rooted tree of* `n` *nodes*. If there are multiple answers, return the answer that occurs last in the given 2D-array.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/12/20/graph1.jpg)

```
Input: edges = [[1,2],[1,3],[2,3]]
Output: [2,3]
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/12/20/graph2.jpg)

```
Input: edges = [[1,2],[2,3],[3,4],[4,1],[1,5]]
Output: [4,1]
```

 

**Constraints:**

- `n == edges.length`
- `3 <= n <= 1000`
- `edges[i].length == 2`
- `1 <= ui, vi <= n`



### 323. Number of Connected Components in an Undirected Graph

Medium

Given `n` nodes labeled from `0` to `n - 1` and a list of undirected edges (each edge is a pair of nodes), write a function to find the number of connected components in an undirected graph.

**Example 1:**

```
Input: n = 5 and edges = [[0, 1], [1, 2], [3, 4]]

     0          3
     |          |
     1 --- 2    4 

Output: 2
```

**Example 2:**

```
Input: n = 5 and edges = [[0, 1], [1, 2], [2, 3], [3, 4]]

     0           4
     |           |
     1 --- 2 --- 3

Output:  1
```

**Note:**
You can assume that no duplicate edges will appear in `edges`. Since all edges are undirected, `[0, 1]` is the same as `[1, 0]` and thus will not appear together in `edges`.



```python
class DSU:
    def __init__(self,n):
        self.par = [i for i in range(n)]
    def find(self, x):
        if self.par[x]!=x:
            self.par[x] = self.find(self.par[x])
        return self.par[x]
    def union(self,x,y):
        xp, yp = self.find(x), self.find(y)
        if xp==yp: return False
        self.par[xp] = yp
        return True
    def compress(self):
        for x in range(len(self.par)):
            self.find(x)
        

class Solution:
    def countComponents(self, n: int, edges: List[List[int]]) -> int:
        dsu = DSU(n)
        for edge in edges:
            dsu.union(*edge)
        dsu.compress()
        
        return len(set(dsu.par))
```



### 1334. Find the City With the Smallest Number of Neighbors at a Threshold Distance

Medium

There are `n` cities numbered from `0` to `n-1`. Given the array `edges` where `edges[i] = [fromi, toi, weighti]` represents a bidirectional and weighted edge between cities `fromi` and `toi`, and given the integer `distanceThreshold`.

Return the city with the smallest number of cities that are reachable through some path and whose distance is **at most** `distanceThreshold`, If there are multiple such cities, return the city with the greatest number.

Notice that the distance of a path connecting cities ***i*** and ***j*** is equal to the sum of the edges' weights along that path.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/01/16/find_the_city_01.png)

```
Input: n = 4, edges = [[0,1,3],[1,2,1],[1,3,4],[2,3,1]], distanceThreshold = 4
Output: 3
Explanation: The figure above describes the graph. 
The neighboring cities at a distanceThreshold = 4 for each city are:
City 0 -> [City 1, City 2] 
City 1 -> [City 0, City 2, City 3] 
City 2 -> [City 0, City 1, City 3] 
City 3 -> [City 1, City 2] 
Cities 0 and 3 have 2 neighboring cities at a distanceThreshold = 4, but we have to return city 3 since it has the greatest number.
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/01/16/find_the_city_02.png)

```
Input: n = 5, edges = [[0,1,2],[0,4,8],[1,2,3],[1,4,2],[2,3,1],[3,4,1]], distanceThreshold = 2
Output: 0
Explanation: The figure above describes the graph. 
The neighboring cities at a distanceThreshold = 2 for each city are:
City 0 -> [City 1] 
City 1 -> [City 0, City 4] 
City 2 -> [City 3, City 4] 
City 3 -> [City 2, City 4]
City 4 -> [City 1, City 2, City 3] 
The city 0 has 1 neighboring city at a distanceThreshold = 2.
```

 

Dijstra

```python
from collections import deque, defaultdict
import heapq
class Solution:
    def findTheCity(self, n: int, edges: List[List[int]], distanceThreshold: int) -> int:
        def dijkstra(start):
            dist = [float('inf')]*n
            dist[start] = 0
            heap = [(0,start)]
            while heap:
                curDist, cur = heapq.heappop(heap)
                if curDist > dist[cur]:
                    continue
                for nei, weight in graph[cur]:
                    newDist = curDist + weight
                    if newDist < dist[nei]:
                        dist[nei] = newDist
                        heapq.heappush(heap, (newDist, nei))
            return sum(1 for d in dist if d<=distanceThreshold)
        
        graph = defaultdict(list)
        for edge in edges:
            graph[edge[0]].append((edge[1], edge[2]))
            graph[edge[1]].append((edge[0], edge[2]))
        
        # print(graph)
        
        res = 0
        curMin = n
        for i in range(n):
            curCount = dijkstra(i)
            if curCount<=curMin:
                curMin, res = curCount, i
        
        return res
```



### 1042. Flower Planting With No Adjacent

Medium

You have `n` gardens, labeled from `1` to `n`, and an array `paths` where `paths[i] = [xi, yi]` describes a bidirectional path between garden `xi` to garden `yi`. In each garden, you want to plant one of 4 types of flowers.

All gardens have **at most 3** paths coming into or leaving it.

Your task is to choose a flower type for each garden such that, for any two gardens connected by a path, they have different types of flowers.

Return ***any** such a choice as an array* `answer`*, where* `answer[i]` *is the type of flower planted in the* `(i+1)th` *garden. The flower types are denoted* `1`*,* `2`*,* `3`*, or* `4`*. It is guaranteed an answer exists.*

 

**Example 1:**

```
Input: n = 3, paths = [[1,2],[2,3],[3,1]]
Output: [1,2,3]
Explanation:
Gardens 1 and 2 have different types.
Gardens 2 and 3 have different types.
Gardens 3 and 1 have different types.
Hence, [1,2,3] is a valid answer. Other valid answers include [1,2,4], [1,4,2], and [3,2,1].
```

**Example 2:**

```
Input: n = 4, paths = [[1,2],[3,4]]
Output: [1,2,1,2]
```

**Example 3:**

```
Input: n = 4, paths = [[1,2],[2,3],[3,4],[4,1],[1,3],[2,4]]
Output: [1,2,3,4]
```





### 547. Number of Provinces

Medium

There are `n` cities. Some of them are connected, while some are not. If city `a` is connected directly with city `b`, and city `b` is connected directly with city `c`, then city `a` is connected indirectly with city `c`.

A **province** is a group of directly or indirectly connected cities and no other cities outside of the group.

You are given an `n x n` matrix `isConnected` where `isConnected[i][j] = 1` if the `ith` city and the `jth` city are directly connected, and `isConnected[i][j] = 0` otherwise.

Return *the total number of **provinces***.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/12/24/graph1.jpg)

```
Input: isConnected = [[1,1,0],[1,1,0],[0,0,1]]
Output: 2
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/12/24/graph2.jpg)

```
Input: isConnected = [[1,0,0],[0,1,0],[0,0,1]]
Output: 3
```



```python
count = 0
n = len(isConnected)
visited  = [False]*n
for i in range(n):
    if not visited[i]:
        stack = [i]
        count += 1
        while stack:
            cur = stack.pop()
            visited[cur] = True
            for j, edge in enumerate(isConnected[cur]):
                if edge and not visited[j]:
                    stack.append(j)
return count
```



### 1376. Time Needed to Inform All Employees

Medium

A company has `n` employees with a unique ID for each employee from `0` to `n - 1`. The head of the company is the one with `headID`.

Each employee has one direct manager given in the `manager` array where `manager[i]` is the direct manager of the `i-th` employee, `manager[headID] = -1`. Also, it is guaranteed that the subordination relationships have a tree structure.

The head of the company wants to inform all the company employees of an urgent piece of news. He will inform his direct subordinates, and they will inform their subordinates, and so on until all employees know about the urgent news.

The `i-th` employee needs `informTime[i]` minutes to inform all of his direct subordinates (i.e., After informTime[i] minutes, all his direct subordinates can start spreading the news).

Return *the number of minutes* needed to inform all the employees about the urgent news.

 

**Example 1:**

```
Input: n = 1, headID = 0, manager = [-1], informTime = [0]
Output: 0
Explanation: The head of the company is the only employee in the company.
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/02/27/graph.png)

```
Input: n = 6, headID = 2, manager = [2,2,-1,2,2,2], informTime = [0,0,1,0,0,0]
Output: 1
Explanation: The head of the company with id = 2 is the direct manager of all the employees in the company and needs 1 minute to inform them all.
The tree structure of the employees in the company is shown.
```

**Example 3:**

![img](https://assets.leetcode.com/uploads/2020/02/28/1730_example_3_5.PNG)

```
Input: n = 7, headID = 6, manager = [1,2,3,4,5,6,-1], informTime = [0,6,5,4,3,2,1]
Output: 21
Explanation: The head has id = 6. He will inform employee with id = 5 in 1 minute.
The employee with id = 5 will inform the employee with id = 4 in 2 minutes.
The employee with id = 4 will inform the employee with id = 3 in 3 minutes.
The employee with id = 3 will inform the employee with id = 2 in 4 minutes.
The employee with id = 2 will inform the employee with id = 1 in 5 minutes.
The employee with id = 1 will inform the employee with id = 0 in 6 minutes.
Needed time = 1 + 2 + 3 + 4 + 5 + 6 = 21.
```

**Example 4:**

```
Input: n = 15, headID = 0, manager = [-1,0,0,1,1,2,2,3,3,4,4,5,5,6,6], informTime = [1,1,1,1,1,1,1,0,0,0,0,0,0,0,0]
Output: 3
Explanation: The first minute the head will inform employees 1 and 2.
The second minute they will inform employees 3, 4, 5 and 6.
The third minute they will inform the rest of employees.
```

**Example 5:**

```
Input: n = 4, headID = 2, manager = [3,3,-1,2], informTime = [0,0,162,914]
Output: 1076
```

 

**Constraints:**

- `1 <= n <= 105`
- `0 <= headID < n`
- `manager.length == n`
- `0 <= manager[i] < n`
- `manager[headID] == -1`
- `informTime.length == n`
- `0 <= informTime[i] <= 1000`
- `informTime[i] == 0` if employee `i` has no subordinates.
- It is **guaranteed** that all the employees can be informed.



+ Max Root-to-leaf sum, each node has single parent: tree

```python
graph = [[] for _ in range(n)]
for i in range(n):
    u,v = manager[i], i
    if u!= -1:
        graph[u].append(v)

def dfs(start):
    maxDist = -1
    for nei in graph[start]:
        maxDist = max(maxDist, dfs(nei))

    if maxDist == -1: return 0
    return maxDist + informTime[start]

return dfs(headID)
```

