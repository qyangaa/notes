### 378. Kth Smallest Element in a Sorted Matrix

Medium

Given a *n* x *n* matrix where each of the rows and columns are sorted in ascending order, find the kth smallest element in the matrix.

Note that it is the kth smallest element in the sorted order, not the kth distinct element.

**Example:**

```
matrix = [
   [ 1,  5,  9],
   [10, 11, 13],
   [12, 13, 15]
],
k = 8,

return 13.
```

+ Analysis : n rows m columns
  + Outer loop: binary search which row k to insert (compare to the left most number) between first row and last row. 
    + Binary search gives the smallest row with number of elements smaller than the first member is >k
  + Inner loop: check whether current number has k numbers smaller than it. 
    + Find insertion point at row 1, then insertion point at row 2 is left 
    + Up to row k, since it is the smallest number at row k, no members downward have smaller number
  + When we find row k, we go to the row before it and binary search again to find the exact number
  + This method is complicated but fast

```python
import bisect

class Solution:
    def kthSmallest(self, matrix: List[List[int]], k: int) -> int:
        self.rows = len(matrix)
        self.columns = len(matrix[0])
        self.matrix = matrix
        
        
        # first biset on the last element on each row to find the first row whose last element has >=k smaller numbers
        l, r = 0, self.rows-1
        totSmaller = 0
        while l<r:
            m=l+(r-l)//2
            totSmaller = self.numSmaller(m)
            if totSmaller>=k:
                r=m
            else:
                l=m+1
    
    
        totSmaller = self.numSmaller(l)
        curNum = self.matrix[l][-1]
        
        # if we are lucky, return last element of current row
        if totSmaller==k: return curNum
        
        #unlucky, binary search between last element of curRow and prevRow to find the correct number. 
        prevRow=l-1
        if l>0:
            l, r = matrix[l-1][-1], matrix[l][-1]
        else:
            l, r = matrix[l][0],  matrix[l][-1]
        
        while l<r:
            m=l+(r-l)//2
            totSmaller = self.numSmallerWNum(prevRow,m)
            if totSmaller>=k:
                r=m
            else:
                l=m+1
        
        # the search result might not be in the array. Do bisect again to find the largest element in array < correct number
        maxRes = 0
        for row in range(prevRow, self.rows):
            index = bisect.bisect(self.matrix[row],l)
            if index>0:
                maxRes = max(self.matrix[row][index-1], maxRes)
        return maxRes
    
    def numSmaller(self,m):
        curNum = self.matrix[m][-1]
        totSmaller = self.columns*(m+1)
        prevIdx= len(self.matrix[0])
        for row in range(m+1, self.rows):
            totSmaller += bisect.bisect(self.matrix[row],curNum,hi=prevIdx)
        return totSmaller
    
    def numSmallerWNum(self, prevRow, num):
        totSmaller = (prevRow+1)*self.columns if prevRow>=0 else 0
        prevIdx= len(self.matrix[0])
        for row in range(prevRow+1, self.rows):
            totSmaller += bisect.bisect(self.matrix[row],num,hi=prevIdx)
        return totSmaller
            
```



+ Min heap method:![img](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/Figures/378/img4.png) ![img](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/Figures/378/img5.png)



### 875. Koko Eating Bananas

Medium

Koko loves to eat bananas. There are `N` piles of bananas, the `i`-th pile has `piles[i]` bananas. The guards have gone and will come back in `H` hours.

Koko can decide her bananas-per-hour eating speed of `K`. Each hour, she chooses some pile of bananas, and eats K bananas from that pile. If the pile has less than `K` bananas, she eats all of them instead, and won't eat any more bananas during this hour.

Koko likes to eat slowly, but still wants to finish eating all the bananas before the guards come back.

Return the minimum integer `K` such that she can eat all the bananas within `H` hours.

 

**Example 1:**

```
Input: piles = [3,6,7,11], H = 8
Output: 4
```

**Example 2:**

```
Input: piles = [30,11,23,4,20], H = 5
Output: 30
```

**Example 3:**

```
Input: piles = [30,11,23,4,20], H = 6
Output: 23
```

 ```python
class Solution:
    def minEatingSpeed(self, piles: List[int], H: int) -> int:
        self.piles = piles
        self.H = H
        l, r = 1, sum(self.piles)
        while l<r:
            m = l+(r-l)//2
            if self.isLegal(m):
                r=m
            else:
                l=m+1
        return l

    def isLegal(self, k):
        return sum([((p-1) // k) + 1 for p in self.piles])<=self.H
 ```



### 33. Search in Rotated Sorted Array

Medium

You are given an integer array `nums` sorted in ascending order (with **distinct** values), and an integer `target`.

Suppose that `nums` is rotated at some pivot unknown to you beforehand (i.e., `[0,1,2,4,5,6,7]` might become `[4,5,6,7,0,1,2]`).

*If `target` is found in the array return its index, otherwise, return `-1`.*

 

**Example 1:**

```
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
```

**Example 2:**

```
Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1
```

**Example 3:**

```
Input: nums = [1], target = 0
Output: -1
```

+ Simpler method:

  + Binary search to find pivot
  + Reindex the array

+ Three cases:

  + When left<right: normal case
  + When left>right, pivot at middle Two cases:
    + Middle>right
    + Left>middle

  ```python
  class Solution:
      def search(self, nums: List[int], target: int) -> int:
          if len(nums)==0: return -1
          
          l, r = 0, len(nums)-1
          while l<r:
              m = l+(r-l)//2
              cur = nums[m]
              if cur==target: return m
              if nums[l]<=nums[r]:
                  if target<cur:
                      r=m
                  else:
                      l=m+1
              elif cur>nums[r]:
                  if target<cur and target>=nums[l]:
                      r=m
                  else:
                      l=m+1
              else:
                  if target>=nums[l] or target<= cur:
                      r=m
                  else:
                      l=m+1
          
          return l if nums[l]==target else -1
  ```

  

### 74 Search a 2D matrix



Write an efficient algorithm that searches for a value in an `m x n` matrix. This matrix has the following properties:

- Integers in each row are sorted from left to right.
- The first integer of each row is greater than the last integer of the previous row.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/10/05/mat.jpg)

```
Input: matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 3
Output: true
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/10/05/mat2.jpg)

```
Input: matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 13
Output: false
```

+ Two binary searches. First along the last element of each row, then along the found row

```python
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        rows = len(matrix)
        columns = len(matrix[0])
        l, r = 0, rows-1
        while l<r:
            m = l+(r-l)//2
            curLast = matrix[m][-1]
            if curLast>= target:
                r=m
            else:
                l=m+1
        # then l is the row we found
        row = l
        l, r = 0, columns-1
        while l<r:
            m = l+(r-l)//2
            cur = matrix[row][m]
            if cur==target: return True
            if cur > target:
                r=m
            else:
                l=m+1
        return matrix[row][l]==target
```



### 4. Median of Two Sorted Arrays

Hard

Given two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively, return **the median** of the two sorted arrays.

**Follow up:** The overall run time complexity should be `O(log (m+n))`.

 

**Example 1:**

```
Input: nums1 = [1,3], nums2 = [2]
Output: 2.00000
Explanation: merged array = [1,2,3] and median is 2.
```

**Example 2:**

```
Input: nums1 = [1,2], nums2 = [3,4]
Output: 2.50000
Explanation: merged array = [1,2,3,4] and median is (2 + 3) / 2 = 2.5.
```

**Example 3:**

```
Input: nums1 = [0,0], nums2 = [0,0]
Output: 0.00000
```

**Example 4:**

```
Input: nums1 = [], nums2 = [1]
Output: 1.00000
```

**Example 5:**

```
Input: nums1 = [2], nums2 = []
Output: 2.00000
```

 

![image-20210130075711964](/home/arkyyang/files/notes/notes/attachments/image-20210130075711964.png)

Solution see: https://zxi.mytechroad.com/blog/algorithms/binary-search/leetcode-4-median-of-two-sorted-arrays/

```python
import sys

class Solution:
    def findMedianSortedArrays(self, nums1: List[int], nums2: List[int]) -> float:
        n1 = len(nums1)
        n2 = len(nums2)
        minVal = -sys.maxsize-1
        maxVal = sys.maxsize
        
        if(n1>n2):
            return self.findMedianSortedArrays(nums2, nums1)
        
        l, r = 0, n1
        k = (n1+n2+1)//2 #middle element+1
        
        while l<r:
            m1 = l+(r-l)//2
            m2 = k-m1
            
            if nums1[m1]<nums2[m2-1]:
                l=m1+1
            else:
                r=m1
        
        m1,m2 = l, k-l
        c1= max( minVal if m1<=0 else nums1[m1-1], 
                minVal if m2<=0 else nums2[m2-1])
        
        if (n1+n2)%2==1: return c1
        
        c2= min(maxVal if m1>=n1 else nums1[m1], 
                maxVal if m2>=n2 else nums2[m2])
        
        
        return (c1+c2)/2
                
```



```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int n1 = nums1.length;
        int n2 = nums2.length;
        if (n1 > n2)
            return findMedianSortedArrays(nums2, nums1);
        
        int k = (n1+n2+1)/2;
        int l = 0;
        int r = n1;
        
        while (l < r){
            int m1 = l + (r - l)/2;
            int m2 = k - m1;
            if (nums1[m1] >= nums2[m2-1]){
                r = m1;
            }else{
                l = m1 + 1;
            }
        }
        
        int m1 = l;
        int m2 = k - l;
        
        int c1 = Math.max(m1 <= 0 ? Integer.MIN_VALUE : nums1[m1-1],
                         m2 <= 0? Integer.MIN_VALUE: nums2[m2-1]);
        
        if ((n1 + n2)  % 2 == 1){
            return c1;
        }
        
        int c2 = Math.min(m1 >= n1 ? Integer.MAX_VALUE : nums1[m1],
                         m2 >= n2? Integer.MAX_VALUE: nums2[m2]);
        
        return (c1 + c2) * 0.5;
        
            
    }
}
```





### 50. Pow(x, n)

Medium

Implement [pow(*x*, *n*)](http://www.cplusplus.com/reference/valarray/pow/), which calculates *x* raised to the power *n* (i.e. xn).

 

**Example 1:**

```
Input: x = 2.00000, n = 10
Output: 1024.00000
```

**Example 2:**

```
Input: x = 2.10000, n = 3
Output: 9.26100
```

**Example 3:**

```
Input: x = 2.00000, n = -2
Output: 0.25000
Explanation: 2-2 = 1/22 = 1/4 = 0.25
```

+ Recursion:

  + ```python
    class Solution:
        def myPow(self, x: float, n: int) -> float:
            if n==1: return x
            if n==0: return 1
            if n<0: return 1/self.myPow(x,-n)
            sqrt = self.myPow(x,n//2)
            if n%2==1:
                return sqrt*sqrt*x
            else:
                return sqrt*sqrt
    ```

  + ```python
    N = n
    if N<0: x,N = 1/x, -N
    ans, curProd = 1, x
    while N>0:
        if N%1==1: ans*=curProd
        curProd*=curProd
        N=N//2
    return ans
    ```

### 367. Valid Perfect Square

Easy

Given a **positive** integer *num*, write a function which returns True if *num* is a perfect square else False.

**Follow up:** **Do not** use any built-in library function such as `sqrt`.

**Example 1:**

```
Input: num = 16
Output: true
```

**Example 2:**

```
Input: num = 14
Output: false
```

 

```python
class Solution:
    def isPerfectSquare(self, num: int) -> bool:
        if num<=1: return True
        l,r = 0, num//2+1
        while l<r:
            m=l+(r-l)//2
            sqr = m*m
            if sqr == num:
                return True
            if sqr>num:
                r=m
            else:
                l=m+1
        return False
```



### 719. Find K-th Smallest Pair Distance

Hard

Given an integer array, return the k-th smallest **distance** among all the pairs. The distance of a pair (A, B) is defined as the absolute difference between A and B.

**Example 1:**

```
Input:
nums = [1,3,1]
k = 1
Output: 0 
Explanation:
Here are all the pairs:
(1,3) -> 2
(1,1) -> 0
(3,1) -> 2
Then the 1st smallest distance pair is (1,1), and its distance is 0.
```


+ Heap: time limite exceeded
```python
class Solution:
    def smallestDistancePair(self, nums: List[int], k: int) -> int:
        nums.sort()
        n = len(nums)
        heap=[]
        for i in range(0, n-1):
            for j in range(i+1, n):
                heapq.heappush(heap, nums[j]-nums[i])
        
        while k>1:
            heapq.heappop(heap)
            k-=1
        return heapq.heappop(heap)
```

+ TODO: watch 

### 287. Find the Duplicate Number

Medium

Given an array of integers `nums` containing `n + 1` integers where each integer is in the range `[1, n]` inclusive.

There is only **one repeated number** in `nums`, return *this repeated number*.

 

**Example 1:**

```
Input: nums = [1,3,4,2,2]
Output: 2
```

**Example 2:**

```
Input: nums = [3,1,3,4,2]
Output: 3
```

**Example 3:**

```
Input: nums = [1,1]
Output: 1
```

**Example 4:**

```
Input: nums = [1,1,2]
Output: 1
```

 

+ Sort: nlog(n), we want to do better than this

+ set: space n, time n

  + ```python
    numsSet = set()
    for i in nums:
        if i in numsSet: return i
        else:
            numsSet.add(i)
    return
    ```

  + 

+ Each index except for one appears among the numbers. The list can be seen as an index-> nums[index] graph. And the repetition can be found at the entrance of the cycle. To find entrance of cycle:

  + ```python
    fast=slow = nums[0]
    while True:
        fast = nums[nums[fast]]
        slow = nums[slow]
        if slow==fast: break
    slow = nums[0]
    while slow!=fast:
        slow = nums[slow]
        fast = nums[fast]
    return fast
    ```

  + 