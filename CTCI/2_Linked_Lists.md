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

