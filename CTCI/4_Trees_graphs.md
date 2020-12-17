# 4 Trees and Graphs

## Trees

### Basic definition

```java
class Node{
    public String name;
    public Node[] children;
}
class Tree{
    public Node root;
}
```



### Types of Trees

+ Binary trees: each node has only 2 children
+ Binary search tree: all left descendants <= cur <= all right descendants
+ Balanced (binary search) tree: $O(\log n)$ insert & find
  + Complete binary trees: every level filled except right most nodes of last level
    ![image-20201123141136429](../attachments/image-20201123141136429.png)
  + Full binary trees: each node has zero or two children
    ![image-20201123141211617](../attachments/image-20201123141211617.png)
  + Perfect binary trees: all levels full (both full and complete), total $2^k -1$ nodes
    ![image-20201123141245865](../attachments/image-20201123141245865.png)

### Binary tree traversal

+ In-order (left, cur, right)

  ```java
  void inOrderTraversal(TreeNode node){
      if(node!=null){
          inOrderTraversal(node.left);
          visit(node);
          inorderTraversal(node.right);
      }
  }
  ```

+ Pre-order: (cur, left, right)

  ```java
  void preOrderTraversal(TreeNode node){
      if(node!=null){
          visit(node);
          preOrderTraversal(node.left);
          preorderTraversal(node.right);
      }
  }
  ```

+ Post-order: (left, right, cur)

  ```java
  void postOrderTraversal(TreeNode node){
      if(node!=null){
          postOrderTraversal(node.left);
          postorderTraversal(node.right);
          visit(node);
      }
  }
  ```

### Binary Heaps

#### Definition (MinHeap)

+ complete binary tree
+ Every node is smaller than all its children
+ root is minimum

#### Operations

+ Insert $O(\log n)$

  ![image-20201123142011718](../attachments/image-20201123142011718.png)

+ Remove Min: $O(\log n)$
  ![image-20201123142109971](../attachments/image-20201123142109971.png)

### Tries (prefix trees)

#### Properties

+ n-ary tree
+ characters stored at each node
+ each path down the tree represent a word
+ start with dummy head
+ null nodes or * indicate complete words
+ Max number of children: ALPHABET_SIZE+1(*)
+ hash table: can look up whether has word, but not whether any word has certain prefix
+ $O(K)$ time look up, $K=$ length of word
+ ![image-20201123142419648](../attachments/image-20201123142419648.png)



## Graphs

### Definition

	+ Directed: with directed edges ; Undirected: two-way edges
	+ Connected Graph: exist a path between every pair of vertices 
	+ Cyclic graph

### Implementation

#### Adjacency List

```java
class Graph{
    public Node[] nodes;
}
class Node{
    public String name;
    public Node[] children;
}
```

Or an array of lists

#### Adjacency Matrices

+ A true value at matrix$[i][j]$ indicates an edge from node i to node j
+ Symmetric for undirected graph

### Graph Search 

#### Depth first search (DFS)

+ Search one branch exhaustively a time

```java
void DFS(Node root){
    if (root==null) return;
    visit(root);
    root.visited = true;
    for each (Node n in root.adjacent){
        if(n.visited==false){
            DFS(n)
        }
    }
}
```

#### Breadth first search (BFS)

+ exhaust all children before going to next layer

```java
void search(Node root){
    Queue queue = new Queue();
    root.marked = true;
    queue.enqueue(root);
    
    while(!queue.isEmpty()){
        Node r = queue.dequeue();
        visit(r);
        foreach (Node n in r.adjacent){
            if (n.marked == false){
                n.marked = true;
                queue.enqueue(n);
            }
        }
    }
}
```

#### Bidirectional search

+ To find shortest path between two nodes
+ Run two simultaneous BFS from each node, find a shortest path when their searches collide
+ Faster than single BFS source can have many children
  + Assume each node has on average k children, and overall k levels
  + 1 BFS: visit $k^d$ nodes $O(k^d)$
  + Bidirectional: collide half-way through $O(k^{d/2})$
  + ![image-20201123144556041](../attachments/image-20201123144556041.png)

## Interview Questions

p253

### 4.1 Route between nodes

Given a directed graph, design an algorithm to find out whether there is a
route between two nodes.

+ Draw Example
+ Brute Force
  + DFS
+ Optimize + Conceptual algorithm walk through
  + DFS is easier to implement, simply with recursion
  + BFS finds shortest path
+ Implement + Test

```java
enum State{ Unvisited, Visited, Visiting; }

boolean search(Graph graph, Node start, Node end){
    if(start==end) return true;
    
    //Queue as common practice to implement BFS
    LinkedList<Node> queue = new LinkedList<Node>();
    
    //mark all nodes unvisited first
    for (Node u: graph.getNodes()){
        u.state = State.Unvisited;
    }
    start.state = State.Visiting;
    queue.add(start);
    Node u;
    while(!queue.isEmpty()){
        u = queue.removeFirst(); //dequeue()
        if(u!=null){
            for(Node v: u.getAdjacent()){
                if(v.state == State.Unvisited){
                    if(v==end){
                        return true;
                    }else{
                        v.state=State.Visiting; //Visiting for nodes in queue
                        q.add(v);
                    }
                }
            }
            u.state = State.Visited // Visited for nodes dequeued
        }
    }
    return false;
    
}
```



### 4.2 Minimal Tree

Given a sorted (increasing order) array with unique integer elements, write an algorithm to create a binary search tree with minimal height.

+ Draw Example

  + [1,2,3,4,5,6,7]

  + ```mermaid
    graph TD;
    	4 --> 2
    	4 --> 6	
    	2 --> 1
    	2 --> 3
    	6 --> 5
    	6 --> 7
    ```

+ Brute Force

+ Optimize + Conceptual algorithm walk through

  + First find the middle element, put it at root
  + Then recursively 
    + find middle element of left part, put at left child
    + find middle element of left part, put at right child
  + Base case:
    + If no element, stop
    + If one element, put at corresponding location

+ Implement + Test

  + ```java
        private static TreeNode createMinimalBST(int arr[], int start, int end){
            if(end<start){
                return null;
            }
            int mid = (start+end)/2;
            TreeNode n = new TreeNode((arr[mid]));
            n.setRightChild(createMinimalBST(arr,start,mid-1));
            n.setRightChild(createMinimalBST(arr,mid+1, end));
            return n;
        }
    ```

  + 



### 4.3 ==List of Depths==

Given a binary tree, design an algorithm which creates a linked list of all the nodes
at each depth (e.g., if you have a tree with depth D, you'll have Dlinked lists).

+ Draw Example

  + ```mermaid
    graph TD;
    	4 --> 2
    	4 --> 6	
    	2 --> 1
    	2 --> 3
    	6 --> 5
    	6 --> 7
    ```

  + [4; 2->6; 1->3->5->7]

+ Brute Force
  
  + Level by level traversal and record each level
+ Optimize + Conceptual algorithm walk through
  + Both BFS and DFS work
    + DFS: pre-order traversal
      + pass level + 1 in each recursive call
    + BFS
      + keep a queue

+ Implement + Test

  ```java
      void createLevelLinkedList(TreeNode root, ArrayList<LinkedList<TreeNode>> lists, int level){
          if(root==null) return;
          LinkedList<TreeNode> list = null;
          if(lists.size()==level){
              list = new LinkedList<TreeNode>();
              lists.add(list);
          }else {
              list = lists.get(level);
          }
          list.add(root);
          createLevelLinkedList(root.left,lists,level+1);
          createLevelLinkedList(root.right,lists,level+1);
      }
  
      public ArrayList<LinkedList<TreeNode>> createLevelLinkedList(TreeNode root){
          ArrayList<LinkedList<TreeNode>> lists = new ArrayList<LinkedList<TreeNode>>();
          createLevelLinkedList(root, lists,0);
          return lists;
      }
      public ArrayList<LinkedList<TreeNode>> answer(TreeNode root) {
          ArrayList<LinkedList<TreeNode>> result = new ArrayList<LinkedList<TreeNode>>();
          LinkedList<TreeNode> current = new LinkedList<TreeNode>();
          if(root!=null){
              current.add(root);
          }
          while (current.size()>0){
              result.add(current);
              LinkedList<TreeNode> parents = current;
              current = new LinkedList<TreeNode>();
              for (TreeNode parent: parents){
                  if(parent.left!=null){
                      current.add(parent.left);
                  }
                  if(parent.right!=null){
                      current.add(parent.right);
                  }
              }
          }
          return result;
      }
  ```

+ Tips

  + Pass lists as argument to recursive calls to keep track of data to be accumulated

### 4.4 Check Balanced

Implement a function to check if a binary tree is balanced. For the purposes of this question, a balanced tree is defined to be a tree such that the heights of the two subtrees of any node never differ by more than one

+ Draw Example

  + Yes: 

    ```mermaid
    graph TD;
    	4 --> 2
    	4 --> 6	
    	2 --> 1
    	2 --> 3
    	6 --> 5
    	6 --> 7
    ```

     

  + Yes:

    ```mermaid
    graph TD;
    	4 --> 2
    	4 --> 6	
    	2 --> 1
    	2 --> 3
    ```

  + No:

    ```mermaid
    graph TD;
    	4 --> 2
    	2 --> 1
    	2 --> 3
    ```

+ Brute Force

  + Traverse with recursion, add height while going down the tree, and output height at each leaf node, see whether they differ more than one

+ Optimize + Conceptual algorithm walk through

  + Check balance in the while traversing
  + Pass an error code (Integer.MIN_VALUE) whenever see unbalanced branch. 

+ Implement + Test

  + ```java
        public int getHeight(TreeNode root){
            if(root==null) return -1;
            return Math.max(getHeight(root.left), getHeight(root.right)) +1;
        }
    
        public boolean isBalanced(TreeNode root)
        {
            if(root==null) return  true;
            int heightDiff = Math.abs(getHeight(root.left) - getHeight(root.right));
            if(heightDiff>1){
                return false;
            }else {
                return isBalanced(root.left) && isBalanced(root.right);
            }
        }
    
        public int checkHeight2(TreeNode root){
            if(root==null) return -1;
            int leftHeight = checkHeight2(root.left);
            if(leftHeight==Integer.MIN_VALUE) return Integer.MIN_VALUE;
            int rightHeight = checkHeight2(root.right);
            if(rightHeight==Integer.MIN_VALUE) return Integer.MIN_VALUE;
            int heightDiff = Math.abs(leftHeight-rightHeight);
            if(heightDiff>1){
                return Integer.MIN_VALUE;
            }else {
                return Math.max(leftHeight,rightHeight)+1;
            }
    
        }
    
        boolean isBalanced2(TreeNode root){
            return checkHeight2(root)!=Integer.MIN_VALUE;
        }
    
    ```

+ Tips:

  + use Integer.MIN_VALUE as error code of function needs to return integer and represent boolean

### 4.5 Validate BST

Implement a function to check if a binary tree is a binary search tree.

+ Draw Example

  + yes

    ```mermaid
    graph TD;
    	4 --> 2
    	4 --> 6	
    	2 --> 1
    	2 --> 3
    	6 --> 5
    	6 --> 7
    ```

  + No

    ```mermaid
    graph TD;
    	4 --> 2
    	4 --> 6	
    	2 --> 1
    	2 --> 5
    	6 --> 8
    	6 --> 7
    ```

    

+ Brute Force

  + in-order traversal, keep all elements in an arraylist and see if it is sorted

+ Optimize + Conceptual algorithm walk through

  + in-order traversal, only check if the current element is >= last seen element
  + min-max: recursive pre-order traversal, return false whenever see order max(left)<=cur<=min(right) is broken

+ Implement + Test

  + ```
        Integer lastPrinted = null;
        public boolean inOrder(TreeNode root){
            if(root==null) return true;
            if(!inOrder(root.left)) return false;
            if(!inOrder(root.right)) return false;
            if(lastPrinted!=null && root.data<lastPrinted) return false;
            lastPrinted = root.data;
    
        }
    
        public boolean minmax(TreeNode root, Integer min, Integer max){
            if(root==null) return true;
            if((min!=null&&root.data<=min)||(max!=null&&root.data>=max)) return false;
            if(!minmax(root.left,min,root.data)||!minmax(root.right,root.data,max))return false;
            return true;
    
        }
    ```

+ Tips:

  + Use variable outside of method to keep track of data while traversing tree
  + use range set by root to check validity of data in branches of BST rather than comparing data within branches to root



### 4.6 Successor

Write an algorithm to find the "next" node (i.e., in-order successor) of a given node in a
binary search tree. You may assume that each node has a link to its parent.

+ Draw Example

  + ```mermaid
    graph TD;
    	4 --> 2
    	4 --> 6	
    	2 --> 1
    	2 --> 3
    	6 --> 5
    	6 --> 7
    ```

  + 5-> 6

  + 2-> 3

  + 3-> 1

+ Brute Force

  + in-order traversal, keep track of last node, return current node as soon as last node is the target node

+ Optimize + Conceptual algorithm walk through

  + Look at structure of tree
    + If has right tree -> left most node of right tree
    + If no right tree -> the first right parent

+ Implement + Test

  + ```java
        int lastNode = Integer.MIN_VALUE;
        int found = Integer.MIN_VALUE;
        public void visit(TreeNode root, Integer target){
    
            //System.out.println(lastNode);
            //System.out.println(root.data);
    
            if(lastNode==target) {
                found = root.data;
            }
            lastNode = root.data;
        }
        public void inOrder(TreeNode root, Integer target){
            if(root!=null){
                inOrder(root.left,target);
                visit(root,target);
                inOrder(root.right,target);
            }
        }
    
        public int answer(TreeNode root, Integer target) {
            inOrder(root,target);
            return found;
        }
    ```

  + If given pointer to node

    ```java
        public TreeNode inorderSucc(TreeNode n){
            if(n==null) return null;
            if(n.right!=null){
                return leftMostChild(n.right);
            }else {
                TreeNode q = n;
                TreeNode x = q.parent;
                while(x!=null && x.left!=q){
                    q = x;
                    x = x.parent;
                }
                return x;
            }
        }
    
        TreeNode leftMostChild(TreeNode n){
            if(n==null){
                return  null;
            }
            while (n.left!=null){
                n=n.left;
            }
            return n;
        }
    ```

    

+ Tips
  + use visit() to abstract any operation during traversal
  + In order successor is: left most child of right children; or first right parent going up parent tree

### 4.7 Build Order

You are given a list of projects and a list of dependencies (which is a list of pairs of
projects, where the second project is dependent on the first project). All of a project's dependencies must be built before the project is. Find a build order that will allow the projects to be built. If there is no valid build order, return an error.

+ Draw Example

  + Input:
      projects: a, b, c, d, e, f
      dependencies: (a, d), (f, b), (b, d), (f, a), (d, c)
    Output: f, e, a, b, d, c

+ Brute Force

  + List all permutations, and check which one satisfies

+ Optimize + Conceptual algorithm walk through

  + build as a directed graph, and see if there is a 

  + ```mermaid
    graph LR;
    	a --> d;
    	f --> b;
    	b --> d;
    	f --> a;
    	d --> c;
    	e
    ```

  + Topological sort:

    + All nodes with no incoming edges
    + Remove outgoing edges from those nodes, repeat until all nodes are cleared
    + If all nodes visited: output
      + Else: return error

  + DFS

    + Start from one node without incoming edge: go forward until see end nodes, add all end nodes to end of build order
    + then trace back: add all parents of end node to start of build order 
    + in the end add start node to start of build order
    + Find other unvisited start node and repeat

+ Implement + Test

  + Topological sort:

    ```java
    public static Graph buildGraph(String[] projects, String[][] dependencies){
            Graph graph = new Graph();
          for (String project: projects){
                graph.getOrCreateNode(project);
            }
            for(String[] dependency: dependencies){
                String first = dependency[0];
                String second = dependency[1];
                graph.addEdge(first,second);
            }
            return graph;
        }
    
        /*
        Projects with no dependencies (no parent)
         */
        public static int addNonDependent(Project[] order, ArrayList<Project> projects, int offset){
            for(Project project: projects){
                if(project.getNumberDependencies()==0){
                    order[offset] = project;
                    offset++;
                }
            }
            return offset;
        }
    
        public static Project[] orderProjects(ArrayList<Project> projects){
            Project[] order = new Project[projects.size()];
            int endOfList = addNonDependent(order, projects, 0);
            int toBeProcessed = 0;
            while (toBeProcessed<order.length){
                Project current = order[toBeProcessed];
                if(current==null){
                    return null;
                }
                ArrayList<Project> children = current.getChildren();
                for(Project child: children){
                    child.decrementDependencies();
                }
                endOfList = addNonDependent(order, children, endOfList);
                toBeProcessed++;
    
            }
            return order;
        }
    
        public static String[] convertToStringList(Project[] projects){
            String[] buildOrder = new String[projects.length];
            for(int i=0; i<projects.length; i++){
                buildOrder[i] = projects[i].getName();
            }
            return buildOrder;
        }
    
        public static Project[] findBuildOrder(String[] projects, String[][] dependencies){
            Graph graph = buildGraph(projects, dependencies);
            return orderProjects(graph.getNodes());
        }
    
        public static String[] buildOrderWrapper(String[] projects, String[][] dependencies){
            Project[] buildOrder = findBuildOrder(projects,dependencies);
            if(buildOrder==null)return null;
            String[] buildOrderString = convertToStringList(buildOrder);
            return buildOrderString;
        }
    ```
    
  + DFS:

    ```java
     public static Graph buildGraph(String[] projects, String[][] dependencies){
            Graph graph = new Graph();
            for (String[] dependency: dependencies){
                String first = dependency[0];
                String second = dependency[1];
                graph.addEdge(first,second);
            }
            return graph;
        }
    
        public static boolean doDFS(Project project, Stack<Project> stack){
            if(project.getState()== Project.State.PARTIAL){
                return false;// Cycle
            }
            if(project.getState()== Project.State.BLANK){
                project.setState(Project.State.PARTIAL);
                ArrayList<Project> children = project.getChildren();
                for(Project child: children){
                    if(!doDFS(child, stack)){
                        return false;
                    }
                }
                project.setState(Project.State.COMPLETE);
                stack.push(project);
            }
            return true;
        }
    
        public static Stack<Project> orderProjects(ArrayList<Project> projects){
            Stack<Project> stack = new Stack<Project>();
            for (Project project: projects){
                if(project.getState()== Project.State.BLANK){
                    if(!doDFS(project,stack)){
                        return null;
                    }
                }
            }
            return stack;
        }
        public static String[] convertToStringList(Stack<Project> projects) {
            String[] buildOrder = new String[projects.size()];
            for (int i = 0; i < buildOrder.length; i++) {
                buildOrder[i] = projects.pop().getName();
            }
            return buildOrder;
        }
    
        public static Stack<Project> findBuildOrder(String[] projects, String[][] dependencies) {
            Graph graph = buildGraph(projects, dependencies);
            return orderProjects(graph.getNodes());
        }
    
        public static String[] buildOrderWrapper(String[] projects, String[][] dependencies) {
            Stack<Project> buildOrder = findBuildOrder(projects, dependencies);
            if (buildOrder == null) return null;
            String[] buildOrderString = convertToStringList(buildOrder);
            return buildOrderString;
        }
    ```

    

+ Tips

  + Put node and graph in separate classes. Put methods to determine number of children/ parent of nodes in node class

### 4.8 First Common Ancestor

Design an algorithm and write code to find the first common ancestor of two nodes in a binary tree. Avoid storing additional nodes in a data structure. NOTE: This is not necessarily a binary search tree.

+ Draw Example

  + ```mermaid
    graph TD;
    	0 --> 4
    	4 --> 2
    	4 --> 6	
    	2 --> 1
    	2 --> 3
    	6 --> 5
    	6 --> 7
    	1 --> 9
    ```

  + (9,8) return 4

+ Brute Force

  + traverse from root, find routes to the two nodes
  + go over routes and find the node before first different node

+ Optimize + Conceptual algorithm walk through

  + Change data structure: store parent pointer in each node. 
    + Traceback method
      + Trace from first node upwards until root, save each node in set visited
      + trace from second node upwards and see when current node is contained in set visited
      + $O(h)$: height of tree
      + Can use depth information to remove the need to store path list -> move deeper node up first and trace both nodes up in the same time.
    + search subtree method
      + go up from first node
      + whenever see a node with unvisited subtree, visit that subtree
      + $O(n)$ : number of nodes
  + If parent pointer is not available
    + Recursive search if nodes in on the same side
      + Returns p, if root's subtree includes p (and not q).
        Returns q, if root's subtree includes q (and not pl.
        Returns null, if neither p nor q are in root's subtree.
        Else, returns the common ancestor of p and q.

+ Implement + Test

  + Traceback

    ```java
        private LinkedHashSet<TreeNode> traceBack(TreeNode node){
            if(node==null){return null;}
            LinkedHashSet<TreeNode> pathToRoot = new LinkedHashSet<TreeNode>();
            TreeNode cur = node;
            while (cur!=null){
                pathToRoot.add(cur);
                cur = cur.parent;
            }
            return pathToRoot;
        }
    
        private TreeNode nodeInPath (TreeNode node, LinkedHashSet<TreeNode> pathToRoot){
            if(node==null) return null;
            if(pathToRoot.contains(node)) return node;
            return nodeInPath(node.parent, pathToRoot);
    
        }
    
        public TreeNode firstCommonAncestor(TreeNode first, TreeNode second){
            LinkedHashSet<TreeNode> firstToRoot = traceBack(first);
            return nodeInPath(second, firstToRoot);
        }
    
        public void test(){
    
            int[] array = {1,2,3,4,5,6,7};
    
            TreeNode main = TreeNode.createMinimalBST(array);
    
            main.left.left.setLeftChild(new TreeNode(9));
            TreeNode first = main.left.left.left;
            TreeNode second = main.right.left;
            System.out.println(firstCommonAncestor(first,second).data);
        }
    ```

  + Traceback answer: move deeper node up first and trace both nodes up in the same time.

    ```java
        TreeNode noMemory(TreeNode p, TreeNode q){
            int delta = depth(p) - depth(q);
            TreeNode shallow = delta>0? q: p;
            TreeNode deep = delta>0? p:q;
            deep = goUpBy(deep, Math.abs(delta));
    
            while (deep!=shallow&& deep!=null && shallow!=null){
                deep=deep.parent;
                shallow=shallow.parent;
            }
            return  deep==null|| shallow==null? null: deep;
        }
    
        TreeNode goUpBy(TreeNode node, int delta){
            while (delta>0 && node!=null){
                node=node.parent;
                delta--;
            }
            return node;
        }
    
        int depth(TreeNode node){
            int depth = 0;
            while (node!=null){
                node=node.parent;
                depth++;
            }
            return depth;
        }
    
    ```

    

+ Tips

  + When dealing with two nodes in the tree, it is usually useful to find the depth of the two nodes first, then move both from the same depth up together

### ==4.9 BST Sequences==

A binary search tree was created by traversing through an array from left to right
and inserting each element. Given a binary search tree with distinct elements, print all possible arrays that could have led to this tree.

+ Draw Example

  ```mermaid
  graph TD;
  	2 --> 1
  	2 --> 3
  ```

  Output: {2,1,3}, {2,3,1}

  ```mermaid
  graph TD;
  	50 --> 20
  	50 --> 60
  	20 --> 10
  	20 --> 25
  	60 --> 70
  	10 --> 5
  	10 --> 15
  	70 --> 65
  	70 --> 80
  ```

  

+ Brute Force

  + Write out all permutations of the numbers and compare which permutations result in same tree as the given tree

+ Optimize + Conceptual algorithm walk through

  + Interpretation: 
    + child elements are always inserted after parent
    + Order of insertion of the two children doesn't matter
  + Recursion
    + Base case: 
      + If no child: return itself.
    + Recursion
      + two children:  two permutations
        + {10,5,15},{10,15,5}
        + {70,65,80},{70,85,65}
        + In the 20 level, 25 can be inserted during any time as long as it is after 20, given the left sibling {10,5,15},
          + {20,}+{25,10,5,15},{10,25,5,15},{10,5,25,15},{10,5,15,25}
        + If the tree in 25 is larger than one node, we need to 'weave' the two subtrees: eg. weaving {1,2,3},{4,5,6}
          + Prepend 1 to all weaves of {2,3} and {4,5,6}
          + Prepend 4 to all weaves of {1,2,3} and {5,6}
          + Use a linked list to implement
      + Only one child, one permutation
        + {60,} + {70,65,80},{70,85,65}

+ Implement + Test

  + Answer:

    ```java
        ArrayList<LinkedList<Integer>> answer(TreeNode node) {
            ArrayList<LinkedList<Integer>> result = new ArrayList<LinkedList<Integer>>();
    
            if(node==null){
                result.add(new LinkedList<Integer>());
                return result;
            }
            LinkedList<Integer> prefix = new LinkedList<Integer>();
            prefix.add(node.data);
    
            ArrayList<LinkedList<Integer>> leftSeq = answer(node.left);
            ArrayList<LinkedList<Integer>> rightSeq = answer(node.right);
    
            for (LinkedList<Integer> left : leftSeq){
                for (LinkedList<Integer> right : rightSeq){
                    ArrayList<LinkedList<Integer>> weaved =
                            new ArrayList<LinkedList<Integer>>();
                    weaveLists(left,right,weaved,prefix);
                    result.addAll(weaved);
                }
            }
            return result;
        }
    
        void weaveLists(LinkedList<Integer> first, LinkedList<Integer> second,
                        ArrayList<LinkedList<Integer>> results, LinkedList<Integer> prefix){
            if (first.size()==0 || second.size() ==0){
                LinkedList<Integer> result = (LinkedList<Integer>) prefix.clone();
                result.addAll(first);
                result.addAll(second);
                results.add(result);
                return;
            }
            int headFirst = first.removeFirst();
            prefix.addLast(headFirst);
            weaveLists(first,second,results,prefix);
            //put back first for operations on the second case
            prefix.removeLast();
            first.addFirst(headFirst);
    
            int headSecond = second.removeFirst();
            prefix.addLast(headSecond);
            weaveLists(first,second,results,prefix);
            prefix.removeLast();
            second.addFirst(headSecond);
        }
    ```

    

+ Tips

  + When recursing over lists and need to return a collection of manipulation between lists, can pass (sublist1, sublist2, resultsCollection, remainingListFromParent) to modify the result when recursing down, and return void. (instead of return results and passing results back)



### 4.10 Check Subtree

T1 and T2 are two very large binary trees, with T1 much bigger than T2. Create an
algorithm to determine if T2 is a subtree of T1. A tree T2 is a subtree of T1 if there exists a node n in Tl such that the subtree of n is identical to T2 . That is, if you cut off the tree at node n, the two trees would be identical.

+ Draw Example

  ```mermaid
  graph TD;
  	50 --> 20
  	50 --> 60
  	20 --> 10
  	20 --> 25
  	60 --> 70
  	10 --> 5
  	10 --> 15
  	70 --> 65
  	70 --> 80
  ```

  The subtree starting from 70 is a subtree, while the following is not a subtree

  ```mermaid
  graph TD;
  	70 --> 80
  	70 --> 65
  ```

  

+ Brute Force

  + Find all subtrees of T1 and compare T2 to each of them

+ Optimize + Conceptual algorithm walk through

  + Improve: If no duplicate number in the tree traverse over T1, whenever see a node with the same value to root of T2, start to traverse them together recursively, check at each step whether T1.left = T2.left and T1.right = T2.right

+ Implement + Test

  + ```java
        boolean subtree(TreeNode T1, TreeNode T2) {
            TreeNode cur = T1;
            if(T2==null) return true;
            if(T1==null) return false;
            if(cur.data!=T2.data){
                return subtree(T1.left, T2) && subtree(T1.right, T2);
            }else {
                return equal(T1, T2);
            }
        }
        boolean equal(TreeNode first, TreeNode second){
            if(first==null && second == null) return true;
            if(first==null || second==null) return false;
            if(first.data!=second.data) return false;
            return equal(first.left,second.left) && equal(first.right,second.right);
        }
    ```

+ $O(n+m)$ in both space and time

### 4.11 Random Node

You are implementing a binary search tree class from scratch, which, in addition
to insert, find, and delete, has a method getRandomNode() which returns a random node from the tree. All nodes should be equally likely to be chosen. Design and implement an algorithm for get RandomNode, and explain how you would implement the rest of the methods.

+ Draw Example

  ```mermaid
  graph TD;
  	50 --> 20
  	50 --> 60
  	20 --> 10
  	20 --> 25
  	60 --> 70
  	10 --> 5
  	10 --> 15
  	70 --> 65
  	70 --> 80
  ```

  

+ Brute Force

  + Choose random: copy all data to an array, and randomly choose from the array
  + Or generate a random index and traverse to get the index
  + Traverse down with certain probabilities of going left or right

+ Optimize + Conceptual algorithm walk through

  + Tune the left-right probability to make the probabilities to each node equal:
    + Cur node: 1/n, left node: $LeftSize*1/n$, right node: $RightSize*1/n$
  + If we know the size at each node, then we can compute the traverse route using a randomly generated index from the size of root (reduces times of random number generation, which can be expensive)

+ Implement + Test

  ```java
  public class RandomTree_4_11 {
      TreeNode root = null;
      public int size(){return root==null? 0: root.getSize();}
      public TreeNode getRandomNode(){
          if(root==null) return  null;
          Random random = new Random();
          int i = random.nextInt(size());
          return root.getIthNode(i);
      }
      public void insertInorder(int value){
          if(root==null){
              root = new TreeNode(value);
          }else {
              root.insertInOrder(value);
          }
      }
  }
  
  class TreeNode{
  
      int data;
      int size = 0;
      public TreeNode left=null;
      public TreeNode right=null;
  
      TreeNode(int data){
          this.data = data;
          this.size = 1;
      }
      public TreeNode getIthNode(int i){
          int leftSize = left==null? 0: left.getSize();
          if (i<leftSize){
              return left.getIthNode(i);
          }else if(i==leftSize){
              return this;
          }else {
              return right.getIthNode(i-(leftSize+1));
          }
      }
      public int getSize() {return size;}
      public int getData() {return data;}
  
      public void insertInOrder(int d){
          if(d<=data){
              if(left==null){
                  this.left = new TreeNode(d);
              }else {
                  left.insertInOrder(d);
              }
          }else {
              if(right==null){
                  this.right = new TreeNode(d);
              }else {
                  right.insertInOrder(d);
              }
          }
          this.size++;
      }
  
      public TreeNode find(int d){
          if(d==data){return this;}
          if(d<data){return left!=null? left.find(d) : null;}
          return right!=null? right.find(d): null;
      }
  }
  ```

  

+ Tips

  + Keep record of size in tree nodes is cheap if implemented with insert
  + Generation of random number can be expensive
  + Easy to find traverse path for node with certain index if size of nodes are known

  

### 4.12 Paths with Sum

You are given a binary tree in which each node contains an integer value (which
might be positive or negative). Design an algorithm to count the number of paths that sum to a given value. The path does not need to start or end at the root or a leaf, but it must go downwards (traveling only from parent nodes to child nodes)

+ Draw Example

  + ```mermaid
    graph TD;
    	10  -->  5
    	10 --> -3
    	5 --> 3
    	5 --> 2
    	3 --> 4
    	3 --> -2
    	2 --> 1
    	-3  --> 11
    	
    ```

  + 18: 1

+ Brute Force

  + Traverse though all paths and count the sum

+ Optimize + Conceptual algorithm walk through

  + We don't need to start from the root every time, repetitive counting

  + Go from top down, store the sum that leads to current node from root, stop counting as soon as sum equals or exceeds the target

  + We only need to remember the sums along the current path for backtrack to save space. Can use recursion in this case

  + countPath(node, target){

    countPath(child, target-node.value)}

+ Implement + Test

  + ```java
        int pathCount = 0;
        void countPaths(TreeNode node, int target)
        {
            if(node==null) {
                if(target==0)pathCount++;
                return;
            }
            if(node.left!=null && node.right!=null){
                countPaths(node.left, target-node.data);
                countPaths(node.right, target-node.data);
                return;
            }
            if(node.left==null){
                countPaths(node.right, target-node.data);
                return;
            }
            if(node.right==null){
                countPaths(node.left, target-node.data);
                return;
            }
            if(target==node.data)pathCount++;
        }
    ```

+ Tips

  + Pull numbers to increment out of recursion for simpler return
  + When dealing with find elements to give a certain sum problem, recurse by decrementing sum by current element

