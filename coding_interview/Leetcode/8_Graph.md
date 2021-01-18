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
graph = defaultdict(set)
def dfs(s,t):
    if s in visited: return False
    visited.add(s)
    if s==t: return True
    return any(dfs(nei, t) for nei in graph[s])
    
for u,v in edges:
    visited=set()
    if u in graph and v in graph and dfs(u,v): return u,v
    graph[u].add(v)
    graph[v].add(u)
    # construct on the go, will return the first edge that completes the cycle
```



+ Solution: Union find. Cycle occurs when dsu can no longer union

```python
dsu=DSU()
for edge in edges:
    if not dsu.union(*edge):
        return edge
```

