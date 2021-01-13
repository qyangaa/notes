## Heaps



### 295 Find Median from Data Stream

Median is the middle value in an ordered integer list. If the size of the list is even, there is no middle value. So the median is the mean of the two middle value.

For example,

```
[2,3,4]`, the median is `3
[2,3]`, the median is `(2 + 3) / 2 = 2.5
```

Design a data structure that supports the following two operations:

- void addNum(int num) - Add a integer number from the data stream to the data structure.
- double findMedian() - Return the median of all elements so far.

 

**Example:**

```
addNum(1)
addNum(2)
findMedian() -> 1.5
addNum(3) 
findMedian() -> 2
```

 

+ Use two heaps:

  ```python
  import heapq
  import sys
  
  class MedianFinder:
  
      def __init__(self):
          """
          initialize your data structure here.
          """
          import heapq
          self.lo = []
          heapq.heapify(self.lo)
          self.hi = []
          heapq.heapify(self.hi)
          
      def addNum(self, num: int) -> None:
          # lo is a max he
          lo = self.lo
          hi = self.hi
          if not lo or -lo[0] < num:
              heapq.heappush(hi,num)
          else:
              heapq.heappush(lo,-num)
          balance_heaps(lo,hi)
  
      def findMedian(self) -> float:
          lo = self.lo
          hi = self.hi
          if not lo and not hi:
              return 0
          if len(lo) == len(hi):
              med = (-lo[0]+hi[0])/2.0
          elif len(lo) > len(hi):
              med = -lo[0]/1.0
          elif len(hi) > len(lo):
              med = hi[0]/1.0
          return med
      def popNum(self, num):
          if num>=self.hi[0]:
              self.hi.remove(num)
              heapq.heapify(self.hi)
              balance_heaps(self.lo,self.hi)
          else:
              self.lo.remove(-num)
              heapq.heapify(self.lo)
              balance_heaps(self.lo,self.hi)
                  
  def balance_heaps(lo,hi):
      while len(lo) - len(hi) > 1:
          heapq.heappush(hi,-heapq.heappop(lo))
      while len(hi) - len(lo) > 1:
          heapq.heappush(lo,-heapq.heappop(hi))       
  ```

  

### 480 Sliding Window Median

Median is the middle value in an ordered integer list. If the size of the list is even, there is no middle value. So the median is the mean of the two middle value.

Examples:

```
[2,3,4]` , the median is `3
[2,3]`, the median is `(2 + 3) / 2 = 2.5
```

Given an array *nums*, there is a sliding window of size *k* which is moving from the very left of the array to the very right. You can only see the *k* numbers in the window. Each time the sliding window moves right by one position. Your job is to output the median array for each window in the original array.

For example,
Given *nums* = `[1,3,-1,-3,5,3,6,7]`, and *k* = 3.

```
Window position                Median
---------------               -----
[1  3  -1] -3  5  3  6  7       1
 1 [3  -1  -3] 5  3  6  7       -1
 1  3 [-1  -3  5] 3  6  7       -1
 1  3  -1 [-3  5  3] 6  7       3
 1  3  -1  -3 [5  3  6] 7       5
 1  3  -1  -3  5 [3  6  7]      6
```

Therefore, return the median sliding window as `[1,-1,-1,3,5,6]`.

+ Use two heap class created in last question

```python
class Solution:
    def medianSlidingWindow(self, nums: List[int], k: int) -> List[float]:
        medianFinder = MedianFinder()
        res=[]
        for i in range(k):
            medianFinder.addNum(nums[i])
        res.append(medianFinder.findMedian())
        for j in range(k,len(nums)):
            medianFinder.popNum(nums[j-k])
            medianFinder.addNum(nums[j])
            res.append(medianFinder.findMedian())
        return res
```



### 692 Top K Frequent Words

iven a non-empty list of words, return the *k* most frequent elements.

Your answer should be sorted by frequency from highest to lowest. If two words have the same frequency, then the word with the lower alphabetical order comes first.

**Example 1:**

```
Input: ["i", "love", "leetcode", "i", "love", "coding"], k = 2
Output: ["i", "love"]
Explanation: "i" and "love" are the two most frequent words.
    Note that "i" comes before "love" due to a lower alphabetical order.
```



**Example 2:**

```
Input: ["the", "day", "is", "sunny", "the", "the", "the", "sunny", "is", "is"], k = 4
Output: ["the", "is", "sunny", "day"]
Explanation: "the", "is", "sunny" and "day" are the four most frequent words,
    with the number of occurrence being 4, 3, 2 and 1 respectively.
```

+ Dictionary: `{word: frequency}`, Heap: `[frequency]` 

  ```python
  import heapq
  
  class Solution:
      def topKFrequent(self, words: List[str], k: int) -> List[str]:
          word2freq=defaultdict()
          for word in words:
              if word not in word2freq:
                  word2freq[word]=1
              else:
                  word2freq[word]+=1
          freqlist=list(word2freq.items())
          topkfreqs = heapq.nsmallest(k,freqlist,lambda x: (-x[1], x[0]))
          return [item[0] for item in topkfreqs]
  ```

  + Use Counter is cleaner but slower

```python
import heapq
import collections

class Solution:
    def topKFrequent(self, words: List[str], k: int) -> List[str]:
        count = list(collections.Counter(words).items())
        topkfreqs = heapq.nsmallest(k,count,lambda x: (-x[1], x[0]))
        return [item[0] for item in topkfreqs]
```



### 502

https://leetcode.com/problems/ipo/