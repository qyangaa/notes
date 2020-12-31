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

### 2.5 Sum Lists

Sum two numbers represented digit by digit by a linked list:

+ Draw Example

  + 1.  reversed order: in: (7-> 1 -> 6) + (5 -> 9 -> 2) ; out: 2 - > 1 - > 9
  + 2. forward order: in: (6 -> 1 -> 7) + (2 -> 9 -> 5); out: 9 - > 1 - > 2

+ Brute Force

  + Reversed order: go over list, create a integer number for each list, and then add the integer, then convert resulting number to list
  + Forward order: reverse both list first, then 

+ Optimize + Conceptual algorithm walk through

  + Smarter way: go over two lists in the same time and add one by one, carry if needed. Add a carry variable
    + for each step: for each current non-null node in list 1 and 2
      + add their values to carry
      + add new node with < 10 value as data
      + change carry to the remainder
  + Forward order:
    + Solution: Pad at the beginning to make lists the same length, then Recursive. Issue: checking length already requires one pass. Can reverse list in the same complexity
    + 

+ Implement + Test

  + ```java
        public Node_single answer(Node_single first, Node_single second) {
            Node_single dummyHead = new Node_single(0);
            Node_single cur = dummyHead;
            int carry = 0;
            while (first != null || second != null || carry>0) {
                cur.next = new Node_single(carry);
                if(first!=null){cur.next.data+= first.data; }
                if(second!=null){cur.next.data+= second.data; }
                if (cur.next.data>=10){
                    carry = 1;
                    cur.next.data-=10;
                }else {
                    carry = 0;
                }
                first = first.next;
                second = second.next;
                cur = cur.next;
            }
    
            return dummyHead.next;
        }
        public void test(){
            //Node_single head = new Node_single
            Integer[] firstList = {7,1,6};
            Integer[] secondList = {5,9,2};
            Node_single first = Node_single.makeList(firstList);
            Node_single second = Node_single.makeList(secondList);
            System.out.println(answer(first,second));
    
        }
    ```

    
    

### 2.6 List palindrome

check if a linked list is a palindrome.

+ Draw Example

  + 0->1-> 2->1->0 yes

+ Brute Force

  + The list must be the same forward and backwards. Reverse the list ($O(n)$), and then go over both lists and compare element by element to see if they are the same.

+ Optimize + Conceptual algorithm walk through

  + Use a stack: get the length of list, push first half of the list into stack one by one, then pop later half one by one to see if they are the same. If odd: ignore middle one. This gives similar run time with reversing list if get length is $O(n)$
  + Recursive: reduce length each time

+ Implement + Test

  + ```java
        public Node_single reverseList(Node_single head){
            Node_single prev = null;
            Node_single cur = head;
            while(cur!=null){
                //have to copy to new node to prevent changing the original list
                Node_single newCur = new Node_single(cur.data);
                newCur.next = prev;
                prev = newCur;
                cur = cur.next;
            }
            return prev;
        }
    
        public Boolean answer(Node_single first) {
            Node_single reversed = reverseList(first);
            Node_single curFirst = first;
            Node_single curReversed = reversed;
            while(curFirst!=null){
                if(curFirst.data != curReversed.data){
                    return false;
                }else{
                    curFirst = curFirst.next;
                    curReversed = curReversed.next;
                }
            }
            return true;
    
    
    
        }
        public void test(){
            //Node_single head = new Node_single
            Integer[] firstList = {0,1,2,1,0};
            Node_single first = Node_single.makeList(firstList);
            System.out.println(answer(first));
    
            Integer[] secondList = {0,1,2,0};
            Node_single second = Node_single.makeList(secondList);
            System.out.println(answer(second));
    
        }
    ```

  + 

### 2.7 Intersection

Given two (singly) linked lists, determine if the two lists intersect. Return the
intersecting node. Note that the intersection is defined based on reference, not value. That is, if the kth node of the first linked list is the exact same node (by reference) as the jth node of the second linked list, then they are intersecting.

+ Draw Example

  + ![image-20201121102234112](../attachments/image-20201121102234112.png)
  + Since only singly linked list, once they intersect, they will converge to one stream
  + Key challenge is to find the intersecting nodes

+ Brute Force

  + Create a hash table of references and store all nodes, check if there is any repetition

+ Optimize + Conceptual algorithm walk through

  + go over both lists, if they have the same end node, then start backtrack. Backtrack:
    + may need to remember every nodes
  + Instead of back track, count length the same time as finding the end node
    + Chop off the excess nodes in the beginning
    + traverse at the same time and check when the nodes intersect

+ Implement + Test

  + ```java
        Node_single trim(Node_single head, int length){
            for(int i=0; i<length; i++){
                head = head.next;
            }
            return head;
        }
        public Node_single answer(Node_single first, Node_single second) {
            Node_single curFirst = first;
            Node_single curSecond = second;
            Node_single preFirst = null;
            Node_single preSecond = null;
            int lenFirst = 0;
            int lenSecond = 0;
            while(curFirst!=null){
                lenFirst++;
                preFirst = curFirst;
                curFirst=curFirst.next;
            }
            while (curSecond!=null){
                lenSecond++;
                preSecond = curSecond;
                curSecond = curSecond.next;
            }
            if(preSecond!=preFirst){return null;}
            int lengthDif = lenFirst-lenSecond;
            if(lengthDif>0){
                first = trim(first,lengthDif);
            }else if(lengthDif<0){
                second = trim(second, -lengthDif);
            }
            
            while(first!=null){
                if(first==second){return  first;}
                first = first.next;
                second = second.next;
            }
            return null;
        }
        public void test(){
            //Node_single head = new Node_single
            Integer[] firstList = {0,1,2,1,0};
            Node_single first = Node_single.makeList(firstList);
            Node_single cur = first;
            for(int i=0; i<3;i++){
                cur=cur.next;
            }
            Integer[] secondList = {0,1,2,0};
            Node_single second = Node_single.makeList(secondList);
            Node_single curSecond = second;
            for (int i=1;i<secondList.length; i++){
                curSecond = curSecond.next;
            }
            curSecond.next = cur;
            System.out.println(first);
            System.out.println(second);
            System.out.println(answer(first,second));
    
        }
    ```



### 2.8 Loop Detection

p235

returns the node at the beginning of the loop

+ Draw Example

  + A - > B - > C - > D - > E - > C [the same C as earlier) : output C

+ Brute Force

  + Store all references in a hash table and return if find repeating reference

+ Optimize + Conceptual algorithm walk through

  + Classical loop detection: two pointer method: FastRunner (move forward 2 step at a time), SlowRunner (one step at a time). Return if they meet
  + Modification of classical loop detection:
    + non-looped part size k, when slow runner enters the loop
      + FastRunner: 2k steps total, k steps into loop, at K = mod(k, loopSize)'th node into loop; loopSize-K steps from SlowRunner; catches SlowRunner 1 step per unit time
      + SlowRunner: k steps total, K steps from FastRunner
      + They meet after (loopSize-K) steps. - collision spot. k = K+M*loopSize. Total steps to collision spot $s = k+K$
      + $K = s-k\\
        k-M*loopSize = s-k\\
        k = \frac{M*loopSize+s}{2}$
      + loopSize can be obtained by stop fastRunner once collide, and run slowRunner until meet again
      + Then we can use loopSize to calculate k and run from start until k+1
  + Solution for faster loopStart finder:
    +  When they collide, move SlowPointer to LinkedListHead. Keep FastPointer where it is
    +  Move SlowPointer and FastPointer at a rate of one step. Return the new collision point.

+ Implement + Test

  + Solution: 

    ```java
    	public static LinkedListNode FindBeginning(LinkedListNode head) {
    		LinkedListNode slow = head;
    		LinkedListNode fast = head; 
    		
    		// Find meeting point
    		while (fast != null && fast.next != null) { 
    			slow = slow.next; 
    			fast = fast.next.next;
    			if (slow == fast) {
    				break;
    			}
    		}
    
    		// Error check - there is no meeting point, and therefore no loop
    		if (fast == null || fast.next == null) {
    			return null;
    		}
    
    		/* Move slow to Head. Keep fast at Meeting Point. Each are k steps
    		/* from the Loop Start. If they move at the same pace, they must
    		 * meet at Loop Start. */
    		slow = head; 
    		while (slow != fast) { 
    			slow = slow.next; 
    			fast = fast.next; 
    		}
    		
    		// Both now point to the start of the loop.
    		return fast;
    	}
    	
    	public static void main(String[] args) {
    		int list_length = 100;
    		int k = 10;
    		
    		// Create linked list
    		LinkedListNode[] nodes = new LinkedListNode[list_length];
    		for (int i = 0; i < list_length; i++) {
    			nodes[i] = new LinkedListNode(i, null, i > 0 ? nodes[i - 1] : null);
    		}
    		
    		// Create loop;
    		nodes[list_length - 1].next = nodes[list_length - k];
    		
    		LinkedListNode loop = FindBeginning(nodes[0]);
    		if (loop == null) {
    			System.out.println("No Cycle.");
    		} else {
    			System.out.println(loop.data);
    		}
    	}
    ```

    



