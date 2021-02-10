### 1094 Car Pooling

You are driving a vehicle that has `capacity` empty seats initially available for passengers. The vehicle **only** drives east (ie. it **cannot** turn around and drive west.)

Given a list of `trips`, `trip[i] = [num_passengers, start_location, end_location]` contains information about the `i`-th trip: the number of passengers that must be picked up, and the locations to pick them up and drop them off. The locations are given as the number of kilometers due east from your vehicle's initial location.

Return `true` if and only if it is possible to pick up and drop off all passengers for all the given trips. 

 

**Example 1:**

```
Input: trips = [[2,1,5],[3,3,7]], capacity = 4
Output: false
```

**Example 2:**

```
Input: trips = [[2,1,5],[3,3,7]], capacity = 5
Output: true
```

**Example 3:**

```
Input: trips = [[2,1,5],[3,5,7]], capacity = 3
Output: true
```

**Example 4:**

```
Input: trips = [[3,2,7],[3,7,9],[8,3,9]], capacity = 11
Output: true
```

+ Brute:

  + An array of distance
  + Add passenger to each distance in [start, end)
  + Return false once exceeds capacity

+ Optimize:

  + Use array of given distance only

  + First onboard all in order, then dropoff all in order, then we have number of people of any given instance

    ```python
    import bisect
    class Solution:
        def carPooling(self, trips: List[List[int]], capacity: int) -> bool:
            # take
            trips.sort(key=lambda x: x[1])
            onBoard=[[0,0]]
            maxOnBoard=0
            for trip in trips:
                onBoard.append([trip[1], onBoard[-1][1]+trip[0]])
            trips.sort(key=lambda x: x[2])
            for trip in trips:
                index = bisect.bisect(onBoard, [trip[2],trip[0]])
                for i in range(index, len(onBoard)):
                    onBoard[i][1]-=trip[0]
            capacities = [item[1] for item in onBoard]
            return max(capacities)<=capacity
    ```

  + We don't need to sort first. We can get all time stamps, and sort the time stamps, and then we have a forward time sequence. 

    ```python
    class Solution:
        def carPooling(self, trips: List[List[int]], capacity: int) -> bool:
            timestamp = []
            for trip in trips:
                timestamp.append([trip[1], trip[0]])
                timestamp.append([trip[2], -trip[0]])
    		
            # this step will sort by [0] then by [1], thus the negative trip[0]s will all come first
            timestamp.sort()
    
            used_capacity = 0
            for time, passenger_change in timestamp:
                used_capacity += passenger_change
                if used_capacity > capacity:
                    return False
    
            return True
            
    ```

    + Bucket sort (actually the original idea): 1000 is not a big number in space

  ```python
  class Solution:
      def carPooling(self, trips: List[List[int]], capacity: int) -> bool:
          timestamp = [0] * 1001
          for trip in trips:
              timestamp[trip[1]] += trip[0]
              timestamp[trip[2]] -= trip[0]
  
          used_capacity = 0
          for passenger_change in timestamp:
              used_capacity += passenger_change
              if used_capacity > capacity:
                  return False
  
          return True
  ```

  



### Merge Intervals

+ Merge Intervals:  greedy, sort by first, then check if second can merge
  $$
  \begin{aligned}
  &\text { For example, }\\
  &\text { Given }[1,3],[2,6],[8,10],[15,18]\\
  &\text { return }[1,6],[8,10],[15,18]
  \end{aligned}
  $$

```python

```

![image-20210115203854174](/home/arkyyang/files/notes/notes/attachments/image-20210115203854174.png)



### 1636. Sort Array by Increasing Frequency

Easy

Given an array of integers `nums`, sort the array in **increasing** order based on the frequency of the values. If multiple values have the same frequency, sort them in **decreasing** order.

Return the *sorted array*.

 

**Example 1:**

```
Input: nums = [1,1,2,2,2,3]
Output: [3,1,1,2,2,2]
Explanation: '3' has a frequency of 1, '1' has a frequency of 2, and '2' has a frequency of 3.
```

**Example 2:**

```
Input: nums = [2,3,1,3,2]
Output: [1,3,3,2,2]
Explanation: '2' and '3' both have a frequency of 2, so they are sorted in decreasing order.
```

**Example 3:**

```
Input: nums = [-1,1,-6,4,5,-6,1,4,1]
Output: [5,-1,4,4,-6,-6,1,1,1]
```

 

```python
from collections import Counter

class Solution:
    def frequencySort(self, nums: List[int]) -> List[int]:
        counter = list(Counter(nums).items())
        counter.sort(key = lambda x: (x[1], -x[0]), reverse=False)
        res = []
        for item in counter:
            res += [item[0]]*item[1]
        return res
        
```



