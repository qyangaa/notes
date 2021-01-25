### 81. Search in Rotated Sorted Array II

Medium

You are given an integer array `nums` sorted in ascending order (not necessarily **distinct** values), and an integer `target`.

Suppose that `nums` is rotated at some pivot unknown to you beforehand (i.e., `[0,1,2,4,4,4,5,6,6,7]` might become `[4,5,6,6,7,0,1,2,4,4]`). 

**Example 1:**

```
Input: nums = [2,5,6,0,0,1,2], target = 0
Output: true
```

**Example 2:**

```
Input: nums = [2,5,6,0,0,1,2], target = 3
Output: false
```



```python
    if not nums: return -1
    l, r = 0, len(nums)-1
    while l<r:

        m=l+(r-l)//2
        mval = nums[m]
        if mval==target:
            return True
        if mval==nums[r]: # we don't know which side it is on, so we can only move linearly
            r=r-1
            continue
        elif mval==nums[l]:
            l=l+1
            continue
        if mval<nums[r]:
            if mval<target<=nums[r]:
                l=m+1
            else:
                r=m
        else:
            if nums[l]<= target < mval:
                r = m
            else:
                l=m+1
    return l<len(nums) and nums[l]==target
```



### 179. Largest Number

Medium

Given a list of non-negative integers `nums`, arrange them such that they form the largest number.

**Note:** The result may be very large, so you need to return a string instead of an integer.

 

**Example 1:**

```
Input: nums = [10,2]
Output: "210"
```

**Example 2:**

```
Input: nums = [3,30,34,5,9]
Output: "9534330"
```

**Example 3:**

```
Input: nums = [1]
Output: "1"
```

**Example 4:**

```
Input: nums = [10]
Output: "10"
```

https://stackoverflow.com/questions/5213033/sort-a-list-of-lists-with-a-custom-compare-function

[3,3,30,30,34,5,9,89,85,78]

+ Key: comparison

```python
    def comparestr(xstr, ystr): # x larger for increasing order
        if xstr==ystr:
            return 0
        xfirst = xstr+ystr
        yfirst = ystr+xstr
        for i in range(len(xfirst)):
            if xfirst[i]>yfirst[i]: return -1
            if xfirst[i]<yfirst[i]: return 1
        return 0

    def compare(x,y):
        return comparestr(str(x),str(y))

    nums = sorted(nums,cmp=compare)
    if not nums: return "0"
    if nums[0]==0: return "0"

    return ''.join([str(n) for n in nums]) 
```

+ Slow, TODO: see how to optimize



### Search for A range: 

[lower, upper] of found number inclusive : TODO: double check

```python
def searchRange(nums, target):
    if not nums: return [-1,-1]
    l,r = 0, len(nums)-1
    found = False
    while l<r: # lower bound
        
        m=l+(r-l+1)//2
        
        if nums[m]==target:
            found = True
        if nums[m]>=target: #lowest index satisfy this
            r=m-1
        else:
            l=m
    if nums[l]!=target: l+=1
    lower = l
    
    l,r=0,len(nums)
    while l<r: # upper bound
        m=l+(r-l)//2
        if nums[m]==target:
            found = True
        if nums[m]<=target:
            l=m+1
        else:
            r=m
    upper = l
    if not found: return [-1,-1]
    return [lower+1, upper]

inputs = [[[6,5,7,7,8,8,10],7,[3,4]],
         [[6,5,7,7,8,8,10],8,[5,6]],
         [[6,5,7,8],7,[3,3]],
         [[6,5,7,8,8],8,[4,5]],
         [[6],6,[1,1]],
          [[6,6],6,[1,2]],
         [[6,5,7,8],3,[-1,-1]]]

for nums,target, sol in inputs:
    print(searchRange(nums,target), sol)
        
```





### 349. Intersection of Two Arrays

Easy

Given two arrays, write a function to compute their intersection.

**Example 1:**

```
Input: nums1 = [1,2,2,1], nums2 = [2,2]
Output: [2]
```

**Example 2:**

```
Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
Output: [9,4]
```

**Note:**

- Each element in the result must be unique.
- The result can be in any order.



+ Not subarray, just number that appear multiple times
+ create a set of one array, go over another array to find 

```python
set1 = set(nums1)
res=set()
for i in nums2:
    if i in set1: res.add(i)
return list(res)

### Built-in function:
return list(set2 & set1)
```

