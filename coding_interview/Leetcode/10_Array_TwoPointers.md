## Array Basics

+ `addr=base_addr+index*sizeof(elem)`
+ same type elements
+ Get, set $O(1)$
+ Remove/ add/ find number $O(n)$



## Two Pointer

### 2 Sum ($O(n\log n)$)

```java
int[] result=new int[2];
Arrays.sort(numbers);
int first=0, second=number.length-1;
while(first<second){
    if(numbers[first]+numbers[second]==target){
        result[0] = numbers[first];
        result[1]=numbers[second];
        return result
    }
    if(numbers[first]+numbers[second]>target){
        second--;
    }
    if(numbers[first]+numbers[second]<target){
        first++;
    }
}
return result
```

### 3 Sum ($O(n^2)$)

```java
for(i=0; i<nums.length; i++){
    int first=i+1, second=nums.length-1, new_target=target-nums[i]
    while(..){...} //Two Sum
}
```

+ for k-sum: $O(n^{k-1})$

### Reverse Array

```java
while(first<end){
    swap(nums, first++, end--)
}
```

+ Follow up:
  + reverse number
  + palindrome

### Odd Even Sort

+ put all odd numbers on to the left and even to the right

```java
while(first<second){
    while(first<second && nums[first]%2==1){
        first++;
    }
    while(first<second && nums[second]%2==0){
        second--;
    }
    swap(nums,first++,second--)
}
```

### Pivot Sort

+ put all numbers `<p` to the left and `>p` to the right

```java
while(first<second){
    while(first<second && nums[first]<=p){
        first++;
    }
    while(first<second && num[second]>=p){
        second--;
    }
    swap(nums,first++,second--)
}
```

### Remove Element

+ Remove in place and return new length

```java
while(first<second){
    while(first<second && nums[first]!=val){
        first++;
    }
    while(first<second && nums[second]==val){
        second--;
    }
    swap(nums, first++, second--)
}
return nums[first] != val? first+1 : first;
```

```java
while(index<len){
    if(nums[index]==val){
        len--;
        nums[index]=nums[len;]
    }else{
        index++;
    }
}
```



### Merge two sorted array

```java
int[] result = new int[arr1.length+arr2.length];
int index=0, index1=0, index2=0;
while(index1<arr1.length && index2<arr1.length){
    if(arr1[index1]<arr2[index2]){
        result[index++]=arr1[index1++];
    }else{
        result[index++]=arr2[index2++];
    }
}
for(int i=index1; i<arr1.length;i++){
    result[index++]=arr1[i];
}
for(int i=index2; i<arr2.length;i++){
    result[index++]=arr2[i];
}
```

+ Merge sorted linked list is the same
+ Merge K sorted array: need to retrieve smallest number of each head one by one. Use heap





### 26 Remove Duplicates from Sorted Array

+ $O(1)$ space. 

+ Scan forward:

  + if different, then end of array moves forward, current end changes to the new number
  + if same: end of array doesn't move
  + Works because: array is sorted, if different from current number, then must be bigger, not seen before

+ | i, end | 0    | 1    | 1    | 2    | 3    | 3    | 4    |
  | ------ | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
  | 0,0    | 0    |      |      |      |      |      |      |
  | 1,1    | 0    | 1    |      |      |      |      |      |
  | 2,1    | 0    | 1    |      |      |      |      |      |
  | 3,2    | 0    | 1    | 2    |      |      |      |      |
  | 4,3    | 0    | 1    | 2    | 3    |      |      |      |
  | 5,3    | 0    | 1    | 2    | 3    |      |      |      |
  | 6,4    | 0    | 1    | 2    | 3    | 4    |      |      |

  

```python
if not num: return 0
end=0
for i in range(len(num)):
    if nums[end] != nums[i]:
        end+=1
        nums[end] = nums[i]
return end+1
```



### 11. Container With Most Water

Medium

Given `n` non-negative integers `a1, a2, ..., an` , where each represents a point at coordinate `(i, ai)`. `n` vertical lines are drawn such that the two endpoints of the line `i` is at `(i, ai)` and `(i, 0)`. Find two lines, which, together with the x-axis forms a container, such that the container contains the most water.

**Notice** that you may not slant the container.

 

**Example 1:**

![img](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/17/question_11.jpg)

```
Input: height = [1,8,6,2,5,4,8,3,7]
Output: 49
Explanation: The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.
```

**Example 2:**

```
Input: height = [1,1]
Output: 1
```

**Example 3:**

```
Input: height = [4,3,2,1,4]
Output: 16
```

**Example 4:**

```
Input: height = [1,2,1]
Output: 2
```

+ find i,j such that `min(i,j)*(j-i)` is maxed
+ Sort, then use the start from right and find max. Slow $O(n\log n)$

```python
class Solution:
    def maxArea(self, height: List[int]) -> int:
        index=[i for i in range(len(height))]
        sort = sorted(list(zip(height, index)))
        minIdx= maxIdx = sort[-1][1]
        maxRes=0
        print(sort)
        for i in reversed(range(len(sort))):
            curVal = sort[i][0]
            curIdx = sort[i][1]
            maxDist = max(abs(curIdx-minIdx), abs(curIdx-maxIdx))
            maxRes = max(curVal*maxDist, maxRes)
            minIdx=min(curIdx, minIdx)
            maxIdx=max(curIdx,maxIdx)
        return maxRes
```

+ Two pointer actually works:

  ```python
  maxRes=0
  l,r = 0, len(height)-1
  while l<r:
      maxRes = max(maxRes, (r-l)*min(height[r], height[l]))
      if height[l]<=height[r]:
          l+=1
      else:
          r-=1
  return maxRes
  ```

  

### 125. Valid Palindrome

Easy

16813462Add to ListShare

Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.

**Note:** For the purpose of this problem, we define empty string as valid palindrome.

**Example 1:**

```
Input: "A man, a plan, a canal: Panama"
Output: true
```

**Example 2:**

```
Input: "race a car"
Output: false
```

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        l,r=0, len(s)-1
        while l<r:
            while l<r and not s[l].isalnum():
                l+=1
            while l<r and not s[r].isalnum():
                r-=1
            if not l<=r: break
            if not s[l].lower()==s[r].lower():
                return False
            l+=1
            r-=1
        return True
```



### 917. Reverse Only Letters

Easy

Given a string `S`, return the "reversed" string where all characters that are not a letter stay in the same place, and all letters reverse their positions.

 

**Example 1:**

```
Input: "ab-cd"
Output: "dc-ba"
```

**Example 2:**

```
Input: "a-bC-dEf-ghIj"
Output: "j-Ih-gfE-dCba"
```

**Example 3:**

```
Input: "Test1ng-Leet=code-Q!"
Output: "Qedo1ct-eeLg=ntse-T!"
```

```python
class Solution:
    def reverseOnlyLetters(self, S: str) -> str:
        s=list(S)
        if len(s)<=1: return S
        l,r=0, len(s)-1
        while l<r:
            while l<r and not s[l].isalpha():
                l+=1
            while l<r and not s[r].isalpha():
                r-=1
            if not l<r: break
            s[l], s[r]=s[r],s[l]
            l+=1
            r-=1
        return ''.join(s)
```

+ Or can only use reverse pointer and add one by one to a new array rather than swap



### 167. Two Sum II - Input array is sorted

Easy

Given an array of integers that is already ***sorted in ascending order\***, find two numbers such that they add up to a specific target number.

The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2.

**Note:**

- Your returned answers (both index1 and index2) are not zero-based.
- You may assume that each input would have *exactly* one solution and you may not use the *same* element twice.

 

**Example 1:**

```
Input: numbers = [2,7,11,15], target = 9
Output: [1,2]
Explanation: The sum of 2 and 7 is 9. Therefore index1 = 1, index2 = 2.
```

**Example 2:**

```
Input: numbers = [2,3,4], target = 6
Output: [1,3]
```

**Example 3:**

```
Input: numbers = [-1,0], target = -1
Output: [1,2]
```

+ Can have duplicated numbers

```python
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        l,r=0, len(numbers)-1
        while l<r:
            if numbers[l]+numbers[r]>target:
                r-=1
            elif numbers[l]+numbers[r]<target:
                l+=1
            else:
                return [l+1, r+1]
```





### 977. Squares of a Sorted Array

Easy

Given an integer array `nums` sorted in **non-decreasing** order, return *an array of **the squares of each number** sorted in non-decreasing order*.

 

**Example 1:**

```
Input: nums = [-4,-1,0,3,10]
Output: [0,1,9,16,100]
Explanation: After squaring, the array becomes [16,1,0,9,100].
After sorting, it becomes [0,1,9,16,100].
```

**Example 2:**

```
Input: nums = [-7,-3,2,3,11]
Output: [4,9,9,49,121]
```

+ Find 0 or split between - and +, split array in 2 half, and do merge

```python
import bisect
class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        split = bisect.bisect(nums,0)
        res=[]
        l,r = split-1, split
        while l>=0 and r<len(nums):
            lval=nums[l]**2
            rval=nums[r]**2
            if lval<=rval:
                res.append(lval)
                l-=1
            else:
                res.append(rval)
                r+=1
        while l>=0:
            res.append(nums[l]**2)
            l-=1
        while r<len(nums):
            res.append(nums[r]**2)
            r+=1
        return res
```



### 992. Subarrays with K Different Integers

Hard

Given an array `A` of positive integers, call a (contiguous, not necessarily distinct) subarray of `A` *good* if the number of different integers in that subarray is exactly `K`.

(For example, `[1,2,3,1,2]` has `3` different integers: `1`, `2`, and `3`.)

Return the number of good subarrays of `A`.

 

**Example 1:**

```
Input: A = [1,2,1,2,3], K = 2
Output: 7
Explanation: Subarrays formed with exactly 2 different integers: [1,2], [2,1], [1,2], [2,3], [1,2,1], [2,1,2], [1,2,1,2].
```

**Example 2:**

```
Input: A = [1,2,1,3,4], K = 3
Output: 3
Explanation: Subarrays formed with exactly 3 different integers: [1,2,1,3], [2,1,3], [1,3,4].
```

+ Brutal with counter: Time Limit Exceeded

```python
res=0

for l in range(0,len(A)-K+1):
    maxc=0
    for r in range(l+K,len(A)+1):
        c = len(Counter(A[l:r]))
        maxc=max(c,maxc)
        if c==K:
            res+=1
        elif c>K: break
    if maxc<K:
        break
return res
```

+ Solution:

  + reduce to problem: subarrays with at most K distinct numbers. then solution is subarrays(K)-subarrays(K-1)

  ```python
  from collections import Counter
  class Solution:
      def subarraysWithKDistinct(self, A: List[int], K: int) -> int:
          def subarrays(k):
              count=[0]*(len(A)+1)
              ans=0
              i=0
              for j in range(len(A)):
                  if count[A[j]]==0: # not seen
                      k-=1  #k: distinct numbers left
                  count[A[j]]+=1
                  j+=1
                  while k<0: #more than k distinct numbers
                      count[A[i]]-=1
                      if count[A[i]]==0: # removed all such number
                          k+=1
                      i+=1
                  ans+=j-i+1
              return ans
          return subarrays(K)-subarrays(K-1)
  ```

  



### 16. 3 Sum Closest

Medium

Given an array `nums` of *n* integers and an integer `target`, find three integers in `nums` such that the sum is closest to `target`. Return the sum of the three integers. You may assume that each input would have exactly one solution.

 

**Example 1:**

```
Input: nums = [-1,2,1,-4], target = 1
Output: 2
Explanation: The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
```

+ First find 2sum closest, the use an outer loop to go over the array

  ```python
  class Solution:
      def threeSumClosest(self, nums: List[int], target: int) -> int:
          nums.sort()
          def twoSumClosest(start, end, t):
              closest = nums[start]+nums[end]
              while start<end:
                  curSum = nums[start]+nums[end]
                  if curSum>t:
                      end-=1
                  elif curSum<t:
                      start+=1
                  else:
                      return t
                  if abs(curSum-t)<abs(closest-t):
                      closest=curSum
              return closest
          
          res = sum(nums[0:3])
          for i in range(len(nums)-2):
              curSum = nums[i]+twoSumClosest(i+1, len(nums)-1,target-nums[i])
              if curSum==target: return target
              if abs(curSum-target)<abs(res-target):
                  res = curSum
          return res
  ```

+ Solution: store diff instead of sum to save time. Combine into one loop so we don't need to check min twice

  ```python
  diff = float('inf')
  nums.sort()
  for i in range(len(nums)):
      lo, hi = i + 1, len(nums) - 1
      while (lo < hi):
          sum = nums[i] + nums[lo] + nums[hi]
          if abs(target - sum) < abs(diff):
              diff = target - sum
          if sum < target:
              lo += 1
          else:
              hi -= 1
      if diff == 0:
          break
  return target - diff
  ```

  





### More

Move zeros

Missing Number

Max Consecutive Ones

Rotate Array