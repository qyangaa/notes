p104

## Linked List

### Implementation

```java
class Node{
    Node next = null;//singly linked list
    int data;
    
    public Node(int d){//constructor
        data=d;
    }
    void appendToTail(int d){
        Node end = new Node(d);
        Node n = this;
        while (n.next!=null){//traverse until the end
            n=n.next;
        }
        n.next=end;
    }
}

Node deleteNode(Node head, int d){
	Node n = head;
    if (n.data=d){
        return head.next;//remove head
    }
    while(n.next!=null){
        if(n.next.data==d){
            n.next = n.next.next;
            return head;
        }
        n=n.next;
    }
    return head
}
```

### 2-pointer/ The "Runner" Technique

Example: Rearrange $a_{1}->a_{2}->\cdots->a_{n}->b_{1}->b_{2}->\cdots->b_{n}$ to $a_{1}->b_{1}->a_{2}->b_{2}->\cdots->a_{n}->b_{n}$ 

+ fast pointer p1 move twice as fast as p2. 
+ When p1 reaches the end, move to pointers back at same pace and each step insert p2 elemnt after p1 position

### Recursive problems

Note: takes $O(n)$ space, $n$ is the depth of recursive call. Can then modify to iterative implementation, which is more complex.



## Interview Questions

p220

### 2.1 Remove duplicates from unsorted linked list

Follow up: what if temporary buffer is not allowed?

+ Draw Example
  + 5,8,4,5,9,4,1,3,6,7,5,4 -> 5,8,4,9,1,3,6,7
+ Brute Force
  + for every element, search elements after it and remove if find same value - $O(n^2)$
+ Optimize + Conceptual algorithm walk through
  + hash set: go over the list, if not visited, put in set, if visited (in set), remove
  + If buffer is not allowed: brute force two pointer
+ Implement + Test

```java
public class Remove_dups_2_1 {
    public Node_single answer(Node_single head) {
        HashSet<Integer> hash_Set = new HashSet<Integer>();
        if(head==null) return null;
        Node_single cur = head;
        hash_Set.add(cur.data);
        while (cur.next!=null){
            System.out.println(hash_Set);
            System.out.println(cur.data +"," + cur.next.data);
            if (hash_Set.contains(cur.next.data)){
                System.out.println("Contains: "+cur.next.data);
                cur.next = cur.next.next;
            }else {
                System.out.println("Adding: "+cur.next.data);
                hash_Set.add(cur.next.data);
                cur = cur.next;
            }



        }
        return head;
    }
    public void test(){
        //Node_single head = new Node_single
        Integer[] input = {5,8,4,5,9,4,1,3,6,7,5,4};
        Node_single head = Node_single.makeList(input);
        System.out.println(answer(head));

    }
```

answer

```java
	public static void deleteDups(LinkedListNode n) {
		HashSet<Integer> set = new HashSet<Integer>();
        //use previous, previous.next.. instead of cur,cur.next, cur.next.next for clarity
		LinkedListNode previous = null;
		while (n != null) {
			if (set.contains(n.data)) {
				previous.next = n.next;
			} else {
				set.add(n.data);
				previous = n;
			}
			n = n.next;
		}
	}
```

### 2.2 Return k'th to last

find the kth (k=1 returns last element) to last element of a singly linked list.

+ Draw Example

  + ```
    5,8,4,5,9,4,1,3,6,7,5,4 ; 5
    3,6,7,5,4
    ```

+ Brute Force

  + Easy if size is known: just remove first (length - k) elements
  + If size is unknown: go over the list to get length $O(n)$ , and remove $O(n)$, so total will still be $O(n)$

+ Optimize + Conceptual algorithm walk through

  + To accelerate, only allow one pass
  + Recursive: call index = printKthToLast(head.next, k) + 1, and return if index==k, one pass
  + Iterative: seems to have 2+ passes, not faster

+ Implement + Test

```java
int printKthToLast(Node_single head, int k){
    if (head==null){
        return 0;
    }
    int index = printKthToLast(head.next, k) + 1;
    if(index==k){
        System.out.println(k + "th to last node is " + head.data);
    }
}
```



### 2.3 Delete Node

(any node but the first and last node, not necessarily the exact middle) of a singly linked list, given only access to that node, no access to head

+ Draw Example
  + 1,2,3,4,5, ref to 3
  + 1,2,4,5
+ Brute Force
  + If have access to head: traverse until cur.next = ref
  + No access to head: copy data from next node to cur node and delete next node
+ Optimize + Conceptual algorithm walk through: nothing to optimize
+ Implement + Test

```java
boolean deleteNode(Node ref){
    if(ref==null || ref.next==null){
        return false;
    }
    ref.data = ref.next.data;
    ref.next = ref.next.next;
    return true
}
```



### 2.4 Partition

partition a linked list around a value x, such that all nodes less than x come before all nodes greater than or equal to x.

The partition element x can appear anywhere in the "right partition"; it does not need to appear between the left and right partitions.

input: Node head, int x

+ Draw Example
  + 3,5,8,5,10,2,1; 5
  + 3,1,2, 10 5, 5, 8
+ Brute Force
  + Go through list, create two arrays - smaller and bigger, and occurrence of data = x nodes. Then create list using these two arrays
+ Optimize + Conceptual algorithm walk through
  + Want to do it in place
  + 3 lists - one smaller one bigger on equal, initiate with dummy head
+ Implement + Test

```java
public Node answer(Node head, int x){
    Node small = new Node(0);
    Node big = new Node(0);
    Node same = new Node(0);
    Node cur = head;
    Node smallCur = small;
    Node bigCur = big;
    Node sameCur = same;
    while(cur!=null){
        if (cur.data<x){
            smallCur.next = cur;
            smallCur = smallCur.next;
        }else if (cur.data>x){
            bigCur.next = cur;
            bigCur = bigCur.next;
        }else{
            sameCur.next = cur;
            sameCur = sameCur.next;
        }
        cur = cur.next;
    }
    smallCur.next = same.next;
    sameCur.next = big.next;
    return small.next;    
}
```



