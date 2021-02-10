### OA Check if a given array contains duplicate elements within k distance

+ Example:

  + ```python
    arr = [1,2,3,4,1,2,3,4]
    k = 3
    return: False
    
    arr = [1,2,3,4,1,2,3,4]
    k = 5
    return: True
    ```

+ ```python
  def checkDuplicates(arr, k):
      if not arr: return True
      if not k: return False
      curWindow = set()
      for i in range(len(arr)):
          print(curWindow)
          cur = arr[i]
          if cur in curWindow: return True
          if len(curWindow)<k:
              curWindow.add(cur)
          else:
              curWindow.remove(arr[i-k])
              curWindow.add(cur)
      
      return False
  ```

```java
static boolean checkDuplicates(int arr[], int k)
    {
        // Creates an empty hashset
        HashSet<Integer> set = new HashSet<>();
 
        // Traverse the input array
        for (int i=0; i<arr.length; i++)
        {
            // If already present n hash, then we found 
            // a duplicate within k distance
            if (set.contains(arr[i]))
               return true;
 
            // Add this item to hashset
            set.add(arr[i]);
 
            // Remove the k+1 distant item
            if (i >= k)
              set.remove(arr[i-k]);
        }
        return false;
    }
```



### OA Count distinct elements in every window of size k

```python
arr = [1,2,1,3,4,2,3]
k = 4
return: [3,4,4,3]
from collections import defaultdict

def countDistinct(arr, k):
    if not arr: return True
    if not k: return False
    curWindow = defaultdict()
    count = []
    for i in range(len(arr)):
        if i >= k:
            rmv = arr[i-k]
            curWindow[rmv] -= 1
            if curWindow[rmv] == 0:
                curWindow.pop(rmv)
        cur = arr[i]
        if cur in curWindow:
            curWindow[cur] += 1
        else:
            curWindow[cur] = 1
        if i>= k-1:
            count.append(len(curWindow))

            
    return count

```

```java
    static int[] countDistinct(int[] arr, int k)
    {
        // Creates an empty hashMap hM
        HashMap<Integer, Integer> hM =
                      new HashMap<Integer, Integer>();
 

        int dist_count = 0;
        int[] result = new int[arr.length - k + 1];
 

        for (int i = 0; i < k; i++)
        {
            if (hM.get(arr[i]) == null)
            {
                hM.put(arr[i], 1);
                dist_count++;
            }
            else
            {
               int count = hM.get(arr[i]);
               hM.put(arr[i], count+1);
            }
        }
        result[0] = dist_count;
 
        // Traverse through the remaining array
        for (int i = k; i < arr.length; i++)
        {

            if (hM.get(arr[i-k]) == 1)
            {
                hM.remove(arr[i-k]);
                dist_count--;
            }
            else // reduce count of the removed element
            {
               int count = hM.get(arr[i-k]);
               hM.put(arr[i-k], count-1);
            }

            if (hM.get(arr[i]) == null)
            {
                hM.put(arr[i], 1);
                dist_count++;
            }
            else // Increment distinct element count
            {
               int count = hM.get(arr[i]);
               hM.put(arr[i], count+1);
            }
            result[i - k + 1] = dist_count; 
        }
        return result;
    }
```





### 138. Copy List with Random Pointer

Medium

A linked list of length `n` is given such that each node contains an additional random pointer, which could point to any node in the list, or `null`.

Construct a [**deep copy**](https://en.wikipedia.org/wiki/Object_copying#Deep_copy) of the list. The deep copy should consist of exactly `n` **brand new** nodes, where each new node has its value set to the value of its corresponding original node. Both the `next` and `random` pointer of the new nodes should point to new nodes in the copied list such that the pointers in the original list and copied list represent the same list state. **None of the pointers in the new list should point to nodes in the original list**.

For example, if there are two nodes `X` and `Y` in the original list, where `X.random --> Y`, then for the corresponding two nodes `x` and `y` in the copied list, `x.random --> y`.

Return *the head of the copied linked list*.

The linked list is represented in the input/output as a list of `n` nodes. Each node is represented as a pair of `[val, random_index]` where:

- `val`: an integer representing `Node.val`
- `random_index`: the index of the node (range from `0` to `n-1`) that the `random` pointer points to, or `null` if it does not point to any node.

Your code will **only** be given the `head` of the original linked list.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2019/12/18/e1.png)

```
Input: head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
Output: [[7,null],[13,0],[11,4],[10,2],[1,0]]
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2019/12/18/e2.png)

```
Input: head = [[1,1],[2,1]]
Output: [[1,1],[2,1]]
```

**Example 3:**

**![img](https://assets.leetcode.com/uploads/2019/12/18/e3.png)**

```
Input: head = [[3,null],[3,0],[3,null]]
Output: [[3,null],[3,0],[3,null]]
```

**Example 4:**

```
Input: head = []
Output: []
Explanation: The given linked list is empty (null pointer), so return null.
```

 

```python
        if not head: return None
        nodedict = defaultdict(None)
        oldnodedict = defaultdict()
        newHead = Node(head.val)
        
        cur = head
        newcur = newHead
        idx = 0
        while cur:
            oldnodedict[cur] = idx
            nodedict[idx] = newcur
            if cur.next:
                newcur.next = Node(cur.next.val)
            cur = cur.next
            newcur = newcur.next
            idx += 1
        
        cur = head
        newcur = newHead
        while cur:
            if cur.random:
                newcur.random = nodedict[oldnodedict[cur.random]]
            cur = cur.next
            newcur = newcur.next
        
        return newHead

```



### 760. Find Anagram Mappings

Easy

Given two lists `A`and `B`, and `B` is an anagram of `A`. `B` is an anagram of `A` means `B` is made by randomizing the order of the elements in `A`.

We want to find an *index mapping* `P`, from `A` to `B`. A mapping `P[i] = j` means the `i`th element in `A` appears in `B` at index `j`.

These lists `A` and `B` may contain duplicates. If there are multiple answers, output any of them.

For example, given

```
A = [12, 28, 46, 32, 50]
B = [50, 12, 32, 46, 28]
```



We should return

```
[1, 4, 3, 2, 0]
```

as `P[0] = 1` because the `0`th element of `A` appears at `B[1]`, and `P[1] = 4` because the `1`st element of `A` appears at `B[4]`, and so on.

```python
        Bmap = defaultdict(list)
        for i, item in enumerate(B):
            Bmap[item].append(i)
        
        res = []
        for item in A:
            res.append(Bmap[item].pop())
        return res
```

