### Linked List Cycle

Return node where cycle begins or null. 

<img src="/home/arkyyang/files/notes/notes/attachments/image-20210115205947945.png" alt="image-20210115205947945" style="zoom:50%;" />

+ Fast 2x, slow 1x. At the point they meet, we find the location relationship. Then put one pointer back to origin, one pointer at the meeting point, go at same speed (1x) until meet, that point is the begin point.

```python
fast, slow = head
while fast and slow:
    if fast.next: 
        fast = fast.next.next
    else:
        return None
    slow = slow.next
    if fast == slow:
        temp = head
        while temp!=slow:
            temp=temp.next
            slow = slow.next
        return slow
```



### Remove Duplicates 2

remove all duplicates and leave only distinct numbers from the original list

+ Examples:
  + 1-2-2-2-3-3: 1
  + 3-4-4-5-6: 3-5-6

```python
if not head: return head
dummy = ListNode(-1)
dummy.next = head
pre = dummy
while pre.next and pre.next.next:
    if pre.next.val == pre.next.next.val:
        lastVal = pre.next.val
        while pre.next and pre.next.val==lastVal:
            pre.next = pre.next.next
    else:
        pre = pre.next
return dummy.next
        
```



### Swap node in pairs

+ Examples
  + 1-2-3-4 : 2-1-4-3

```python
dummy = ListNode(-1)
dummy.next = head
pre = dummy
while pre.next and pre.next.next:
    first, second = pre.next, pre.next.next
    pre.next, second.next, first.next= second, first, second.next
    pre = first
return dummy.next
```

### Merge Sorted List

```python
dummy = ListNode(-1)
cur = dummy
while head1 and head2:
    if head1.val<head2.val:
        cur.next = head1
        head1=head1.next
    else:
        cur.next = head2
        head2=head2.next
    cur = cur.next
if head1:
    cur.next = head1
elif head2:
    cur.next = head2
return dummy.next
```







### Exercises

+ 2. Add two numbers

     ```python
     class Solution(object):
         def addTwoNumbers(self, l1, l2):
             """
             :type l1: ListNode
             :type l2: ListNode
             :rtype: ListNode
             """
             c1, c2 = l1,l2
             carry = 0
             dummy = ListNode(-1)
             cres = dummy
             while c1 and c2:
                 curSum = c1.val+c2.val+carry
                 carry = curSum//10
                 cres.next = ListNode(curSum%10)
                 c1, c2, cres = c1.next, c2.next, cres.next
             while c1:
                 curSum = c1.val+carry
                 carry = curSum//10
                 cres.next = ListNode(curSum%10)
                 c1, cres = c1.next, cres.next
             while c2:
                 curSum = c2.val+carry
                 carry = curSum//10
                 cres.next = ListNode(curSum%10)
                 c2, cres = c2.next, cres.next
             if carry>0:
                 cres.next=ListNode(carry)
             return dummy.next
     ```

     

+ Remove linked list elements

  ```python
  class Solution(object):
      def removeElements(self, head, val):
          """
          :type head: ListNode
          :type val: int
          :rtype: ListNode
          """
          if not head: return head
          dummy = ListNode(-1)
          dummy.next = head
          cur = dummy
          while cur and cur.next:
              while cur.next and cur.next.val == val:
                  cur.next = cur.next.next
              cur = cur.next
          return dummy.next
  ```

  + Two pointer is faster:

    ```python
            dummy = ListNode(0)
            dummy.next = head
            
            prev, curr = dummy, head
            while curr:
                if curr.val == val:
                    prev.next = curr.next
                else:
                    prev = curr
                curr = curr.next
            
            return dummy.next
    ```

    

+ Odd even linked list

  ```python
  class Solution:
      def oddEvenList(self, head: ListNode) -> ListNode:
          if not head or not head.next: return head
          h1 = c1 = head
          h2 = c2 = head.next
          while c1 and c2:
              c1.next = c2.next
              temp, c1 = c1, c1.next
              if c1:
                  c2.next = c1.next
                  c2 = c2.next
              else:
                  temp.next = h2
                  return h1
          c1.next = h2
          return h1
  ```

  



+ 206 reverse Linked List

  ```python
  class Solution:
      def reverseList(self, head: ListNode) -> ListNode:
          if not head or not head.next: return head
          p, c = None, head
          while c:
              c.next, p, c = p, c, c.next
          return p
  ```

  

+ 141 Linked List Cycle

  ```python
  class Solution:
      def hasCycle(self, head: ListNode) -> bool:
          slow = fast = head
          while fast:
              if not fast.next:
                  return False
              slow, fast = slow.next, fast.next.next
              if slow==fast: return True
          return False
  ```

  

+ 23 Merge k sorted lists:

  ```python
  import heapq
  class Solution:
      def mergeKLists(self, lists: List[ListNode]) -> ListNode:
          n = len(lists)
          heap = []
          while not all(v is None for v in lists):
  
              for i,cur in enumerate(lists):
                  if cur: 
                      heapq.heappush(heap, cur.val)
                      lists[i] = cur.next
                      
          dummy = ListNode(-1)
          cur = dummy
          while heap:
              cur.next = ListNode(heapq.heappop(heap))
              cur = cur.next
          return dummy.next
  ```

  + extract from heap on the go. Use `id` to prevent heapq comparison error. This is not faster than first solution (do two comparisons), and takes much more space (store entire object in heap)

    ```python
    head = cur = ListNode(-1)
    heap = []
    for l in lists:
        if l:
            heapq.heappush(heap, (l.val,id(l), l))
    while heap:
        val,curid, node = heapq.heappop(heap)
        cur.next = ListNode(val)
        cur = cur.next
        node = node.next
        if node:
            heapq.heappush(heap, (node.val,id(node), node))
    return head.next
    ```

+ Insertion sort List

  ```python
  import sys
  class Solution:
      def insertionSortList(self, head: ListNode) -> ListNode:
          if not head or not head.next: return head
          dummy = ListNode(-sys.maxsize-1)
          dummy.next = head
          p, c = head, head.next
          while c:
              s = dummy
              if p.val<=c.val:
                  p,c=c,c.next
                  continue
              while s.next and s.next!=c:
                  if s.val<c.val<=s.next.val:
                      s.next, c.next,p.next = c, s.next, c.next
                      break
                  s = s.next
              c = p.next
          return dummy.next
  ```

  + We can also take the node out, then put it back to the position