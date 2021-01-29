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

