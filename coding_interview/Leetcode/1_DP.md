https://leetcode.com/list/xlern30i/



### 5 Longest Palindromic Substring

Given a string `s`, return *the longest palindromic substring* in `s`.

+ Examples:
  + s="babad", out: "bab", or "aba"
  + s="bccd", out:"cc"
  + s="a", out: "a"
+ First thought:
  + recusion:
    + base case: empty string: null. one char: return s
    + recursion: longestPal(s) = max(maxpalLength(s)+longestPal(s[1:])) $O(n)$
    + maxpalLength(s) checks the length of longest palLength starting from first char in s ($O(n^2)$)
    + Total $O(n^3)$
+ Optimization:
  + isPal:
    + check if in array
    + if(end<start): return false
    + if(end-start<=1): return true
    + if(s[start]==s[end]) `isPalArr[start][end]= isPal(s[start+1:end-1])+2` 
    + else `isPalArr[start][end]= false`
    + Storage
      + once we find a pal, we can store in `isPalArr[start][end]= true`
  + Then we calculate all start-end pair, ~$O(n^2)$

+ Implementation: time limit exceeded

```python
class MaxPalindrome:
    def longestPalindrome(self, s: str):
        self.s = s
        self.length = len(s)
        self.maxPalLength = -1
        self.longestPal = ""
        self.isPalArr = [[-1 for x in range(self.length)] for y in range(self.length)]  # -1: not visited, 0: false,
        # 1: true
        start = 0
        while self.length - start > self.maxPalLength:
            self.longestPalindromFromStart(start)
            start += 1
        print(np.array(self.isPalArr))
        return self.longestPal

    def longestPalindromFromStart(self, start):
        end = self.length-1
        while end >= start:
            if self.isPal(start, end) == 1: break
            end -= 1
        if end - start > self.maxPalLength:
            self.maxPalLength = end - start
            self.longestPal = self.s[start:end+1]
        return

    def isPal(self, start, end):
        if self.isPalArr[start][end] > -1: return self.isPalArr[start][end]
        if end < start:
            self.isPalArr[start][end] = 1
            return 1
        if end - start == 0:
            self.isPalArr[start][end]=1
            return 1
        if self.s[start] == self.s[end]:
            self.isPalArr[start][end] = self.isPal(start + 1, end - 1)
            return self.isPalArr[start][end]
        else:
            self.isPalArr[start][end] = 0
            return 0

```

+ Going over array multiple times is redundant, deal with the matrix directly

```python
class Solution:
    def longestPalindrome(self, string):
        # initialize 2-D table for storing results
        n = len(string)
        results = [[False] * n for i in range(n)]
        x, y = 0, 0  # start, end of longest palindromic substring so far
        # every 1-letter substring is a palindrome
        for i in range(n):
            results[i][i] = True

        # 2-letter substring(i, j) is a palindrome if string[i] == string[j]
        for i in range(n - 1):
            if string[i] == string[i + 1]:
                results[i][i + 1] = True
                # change longest palindrome to the 1st 2-letter palindrome
                if not x and not y:
                    x, y = i, i + 1

        # results[i][j] = True if results[i + 1][j - 1] == True
        # and string[i] == string[j]
        for k in range(2, n):
            for i in range(n - 2):
                j = i + k
                # break the loop if it exceeds the boundaries of the matrix
                if j == n:
                    break
                # check if current substring is a palindrome
                if results[i + 1][j - 1] and string[i] == string[j]:
                    results[i][j] = True
                    # len(substring(i, j)) > len(substring(x, y))
                    # this way we always choose 1st longest palindrome
                    if j - i > y - x:
                        x, y = i, j

        return string[x:y + 1]
```



+ Tips

  + Check if is palindrome:

    ```python
    if isPal(start+1, end-1) and s[start]==s[end]:
        return true
    ```

  + Find palindrome substrings with Recursion:

    ```python
    def findPalindrome(self, s:str):
        if(len(s)==1): 
            print(s)
            return True
        if(len(s)==2): 
            if(s[0]==s[1]):
                print(s)
                return True
            else:
                return False
        if(s[0]==s[-1] and self.findPalindrome(s[1:len(s)-1])):
            print(s)
            return True
        else:
            return False
    ```

    

  + Find palindrome substrings with DP: 

    + DP always starts from the subproblems and expands to bigger problems

      + cases in recursion are
        + len = 1
        + len = 2
        + len > 2
      + Then grow len is the direction of expansion
      + And going over the array is the direction of traverse

    + DP: 

      ```python
      for problem_size in direction_of_expansion:
          for n in direction_of_traverse:
              storageArray[problem] = f(storageArray[sub_problem])
      ```

    + (organized by `lenPal`: length of palindrome). 

    + $O(n^2)$ in time and space.

    ```python
    def findPalindrome(self, s: str):
        length = len(s)
        isPalArr=[[False for i in range(length)] for j in range(length)]
        for i in range(length): # lenPal==1
            isPalArr[i][i]=True
        for i in range(length-1): # lenPal==2
            if s[i]==s[i+1]:
                isPalArr[i][i+1]=True
        for lenPal in range(2,length): # lenPal>2
            for start in range(length-2):
                end = start+lenPal
                if(end==length): break
                if isPalArr[start+1][end-1] and s[start]==s[end]:
                    isPalArr[start][end]=True
                    print(s[start:end+1])
        return
    ```

  + Find palindrome substrings with center expansion: 

    + each palindrome is a sub-palindrome centered at the same location expanded on both side by 1

  + iterate over chars as center location, note cases when center is a char (odd length) and center is between chars (even length)

    + DP without storage if only need to return extrema:

      ```python
      for n in direction_of_traverse:
          for problem_size in direction_of_expansion:
              expanded_value = expand(sub_problem)
      ```

      

    + $O(n^2)$ time, $O(1)$ space.

    ```python
    def findPalindromeExpand(self, s: str):
        length = len(s)
      if length<1: return ""
        for i in range(length):
            oddLength = self.expandAroundCenter(s,i,i)
            evenLength = self.expandAroundCenter(s,i,i+1)
            
    def expandAroundCenter(self, s, left, right):
        L, R = left, right
        while L>=0 and R<len(s) and s[L]==s[R]:
            L-=1
            R+=1
            print(s[L:R+1])
        return R-L-1 #note here we expand both way, need to -1
    ```

    

### 53 Maximum Subarray

Given an integer array `nums`, find the contiguous subarray (containing at least one number) which has the largest sum and return *its sum*.

**Follow up:** If you have figured out the `O(n)` solution, try coding another solution using the **divide and conquer** approach, which is more subtle.

+ Example

  + `[-2,1,-3,4,-1,2,1,-5,4]` -> 6
  + `[1]` -> 1
  + `[0]` -> 0
  + `[-5]`-> -5
  + `1<=len(nums)<=2e4`

+ Analysis

  + Go over the array, store sum up to current index. If previous sum<0, reset sum to 0 and start from current index. Keep record of largest sum ever seen

+ Implementation [correct]

  ```python
  def maxSum(nums):
      maxsum = nums[0]
      cursum = nums[0]
      if len(nums)==1:
          return maxsum
      for i in range(1,len(nums)):
          if cursum<=0:
              cursum=0
          cursum+=nums[i]
          maxsum = max(cursum, maxsum)
      return maxsum
      
  ```

+ Divide and conquer: slower

  + find max in to halves
  + middle case: start at middle, expand on two sides, and record the maximum seen.

+ Tips

  + Find max sum of contiguous subarray:

    ```python
    maxsum = nums[0]
    cursum = nums[0]
    for i in range(1,len(nums)):
            if cursum<=0: #set to 0 if previous sum<0
                cursum=0
            cursum+=nums[i]
            maxsum = max(cursum, maxsum)
    ```



### 55 Jump Game

Given an array of non-negative integers `nums`, you are initially positioned at the **first index** of the array.

Each element in the array represents your maximum jump length at that position.

Determine if you are able to reach the last index.

+ Examples

  + `nums = [2,3,1,1,4]`: true
    + 1, 3
  + `nums = [3,2,1,0,4]`: false
    + always get to nums[3] and cannot move forward

+ Analysis

  + Recursion

    + Contraction direction: from top of array to the end

    ```python
    def canReach(index, s):
        available_jumps = list(range(s[0]+1))
        for jump in available_jumps:
            if canReach(index+jump,s):
                return True
        return False
    ```

    + Store visited

    ```python
        def canJump(self, nums: List[int]) -> bool:
            self.iterations = 0
            self.visited = [-1 for _ in range(len(nums))]
            return self.canReachFrom(0,nums)
    
        def canReachFrom(self,index, s):
            self.iterations+=1
            if self.visited[index] > -1:
                return True if self.visited[index] == 1 else False
            if s[index]>=len(s)-index-1:
                self.visited[index] = 1
                return True
            max_jump = min(s[index], len(s)-index-1)
            available_jumps = list(range(1,max_jump+1))
            for jump in available_jumps:
                if self.canReachFrom(index + jump, s):
                    return True
            self.visited[index] = 0
            return False
    ```

    

  + DP: reversed jump [slower]

    + Expansion direction: from end of array to top
    + Traverse direction: reversed jump starts from end location of last successful reversed jump
    + Storage: visitedIndexs = set()
    + This is actually slower than recursion because in recursion we only need to test available_jumps, but in reversed jump we need to test all possible jumps

    ```python
    def canReach(nums):
        if len(nums) <= 1: return True
        curIndex = len(nums) - 1
        startIndexs = [curIndex]
        while len(startIndexs) != 0:
            curIndex = startIndexs.pop()
            for reverseJump in range(1, curIndex+1):
                endIndex = curIndex - reverseJump
                if nums[endIndex] >= reverseJump:
                    if endIndex == 0: return True
                    startIndexs.append(endIndex)
        return False
                    
    ```

  + DP: turn recursion into iteration $O(n^2)$

    ```python
        def canReach(self, nums):
            self.visited = [-1 for _ in range(len(nums))]
            self.visited[-1] = 1
            for i in range(len(nums)-1):
                max_jump=min(i+nums[i],len(nums)-1)
                self.visited[i] = 0
                for jump in range(i+1,max_jump+1):
                    print(i, jump)
                    if self.visited[jump] == 1:
                        self.visited[i] = 1
                        return True
            return self.visited[0]==1
    ```

  + Greedy:

    + We only need one good jump position
    + Iterate from right to left: once we see a position with `num[i]>len(num)-i-1`, this is a good position
    + Find the right most good jump position R. If any position to the left L is also a good position, that that position must be able to jump to R because `nums[L]>R-L`
    + Then start from this position and go back

    ```python
    def canJump(self, nums):
        end = len(nums)-1
        start = end-1
        while start >= 0:
            if nums[start]>=end-start:
                end=start
            start-=1
        return end==0
    ```

  

### 62 Unique Paths

A robot is located at the top-left corner of a `m x n` grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

How many possible unique paths are there?

+ Example:

  + ![image-20210102091031700](../../attachments/image-20210102091031700.png)
  + `m=3, n=2` -> 3
  + `m=7,n=3`-> 28
  + `m=3, n=3`-> 6

+ Analysis

  + `[m,n]` and `[n,m]` gives same result

  + Base cases: 

    + `[1,1]`: 0
    + `[2,1],[1,2]`: 1
    + `[1,1]`: `f[2,1]+f[1,2]`

  + Recursion:

    ```python
    numPaths(m,n):
        return numPaths(m-1,n)+numPaths(m,n-1)
    ```

  + Memorization:

    ```python
    memo = {(m,n):num_paths}
    numPaths(m,n):
        if (m,n) or (n,m) in memo.keys(), return memo(m,n) or memo(n,m)
    ```

  + DP

    + direction of expansion: `m++, n++`

    ```python
    def numPaths(m,n):
        memo={(m,n): num_paths}
        memo[(1,1)]=0
        memo[(2,1)]=1
        memo[(1,2)]=1
        i = 2
        while i<=m:
            j = 2
            while j<=n:
                memo[(i,j)]=memo[(i-1,j)]+memo[(i,j-1)]
                j+=1
            i+=1
        return memo[m,n]
        
    ```

  + For each i, we only use the memorization from memo[(i-1)] and memo[(i)]. No need to store all rows

    + Note: because we need to use a full row as memorization, we must initialize a full row

    ```python
    def uniquePaths(m,n):
            if m==n==1: return 1
            memoprev, memocur = [1 for _ in range(n)], [1 for _ in range(n)]
            memoprev[0]=0
            memocur[0]=1
            i = 1
            while i < m:
                j = 1
                while j < n:
                    memocur[j] = memoprev[j] + memocur[j - 1]
                    j += 1
                memoprev = memocur
                memocur[0]=1
                i += 1
            return memocur[-1]
    ```

  + Can also use factorial to calculate directly:

    + in total we need to have m-1 steps down, n-1 steps right
    + count all unique combinators (steps down are no different from each other, same for right)
    + $\frac{(m-1+n-1)!}{(m-1)!(n-1)!}$

    ```python
    def uniquePaths(self, m: int, n: int) -> int:
        from math import factorial as fact
        return int(fact((m-1) + (n-1))/(fact(m-1) * fact(n-1)))
    ```

    

### 70 Climbing Stairs

You are climbing a staircase. It takes `n` steps to reach the top.

Each time you can either climb `1` or `2` steps. In how many distinct ways can you climb to the top?

+ Examples:

  + `n=2 -> 2`
  + `n=3 -> 3`

+ Analysis

  + Base case:

    + `n=0 -> 1` 
    + `n=1 -> 1`
    + `n=2 -> 2`

  + recursion

    ```python
    def climbStairs(n):
        return climbStairs(n-1)+climbStairs(n-2)
    ```

  + Memorization:

    ```python
    memo=[0]*n
    def climbStairs(n):
        if memo[n]>0: return memo[n]
        return climbStairs(n-1)+climbStairs(n-2)
    ```

  + DP:

    ```python
    def climbStairs(n):
        if basecase: return basecase
        memo=[0]*n
        memo[0]=1
        memo[1]=1
        i = 2
        while i<=n:
            memo[i]=memo[i-1]+memo[i-2]
        return memo[-1]
    ```

  + DP save space: we only use 1 and 2 steps ahead, no need to store the whole array

    ```python
    def climbStairs(n):
        if n<=1: return 1
        prev1=prev2=1
        cur=prev1+prev2
        i = 2
        while i<=n:
            cur=prev1+prev2
            prev1, prev2 = cur, prev1
            i+=1
        return cur
    ```

+ Tips:

  + climbing stairs problem, Fibonacci problem, all use one pass method:

    ```python
    while i<=n:
        cur=prev1+prev2
        prev1, prev2 = cur, prev1
        i+=1
    ```



### 91 Decode ways

A message containing letters from `A-Z` can be **encoded** into numbers using the following mapping:

```
'A' -> "1"
'B' -> "2"
...
'Z' -> "26"
```

To **decode** an encoded message, all the digits must be mapped back into letters using the reverse of the mapping above (there may be multiple ways). For example, `"111"` can have each of its `"1"`s be mapped into `'A'`s to make `"AAA"`, or it could be mapped to `"11"` and `"1"` (`'K'` and `'A'` respectively) to make `"KA"`. Note that `"06"` cannot be mapped into `'F'` since `"6"` is different from `"06"`.

Given a **non-empty** string `num` containing only digits, return *the **number** of ways to **decode** it*.

The answer is guaranteed to fit in a **32-bit** integer.

+ Example:

  + ```
    Input: s = "12"
    Output: 2
    Explanation: "12" could be decoded as "AB" (1 2) or "L" (12).
    ```

  + ```
    Input: s = "226"
    Output: 3
    Explanation: "226" could be decoded as "BZ" (2 26), "VF" (22 6), or "BBF" (2 2 6).
    ```

  + ```
    Input: s = "0"
    Output: 0
    Explanation: There is no character that is mapped to a number starting with 0. The only valid mappings with 0 are 'J' -> "10" and 'T' -> "20".
    Since there is no character, there are no valid ways to decode this since all digits need to be mapped.
    ```

  + ```
    Input: s = "1"
    Output: 1
    ```

+ Analysis

  + Base cases

    + `len(s)==1`:
      + `s=="0"->0`
      + else: ->1
    + `len(s)==2`:
      + `if 11<=int(s)<=26 && s!="20"`-> 2
      + else: 1 or 0

  + Recursion:

    + each step, check if last 1 and last 2 characters give legal encoding

    ```python
    def numDecodings(s):
        return isValid(s[-1])*numDecodings(s[:-1])+isValid(s[-2:])*numDecodings(s[:-2])
    
    def isValid(s):
        if len(s)==0: return 0
        if len(s)==1:
            return 0 if s=="0" else 1
        if len(s)==2:
            if int(s)>=10 and int(s)<=26: return 1
            else: return 0
            
    
    ```

  + Recursion with memorization:

    ```python
    memo={s: num_decodings}
    ```

  + DP: expand from last 1 and 2 characters

    ```python
    def numDecodings(s):
        if basecase: return basecase
        memo={idx: num_decodings}
        l = len(s)-1
        memo[l] = isValid(s[l])
        memo[l-1] = isValid(s[l-1:])+isValid(l)*isValid(l-1)
        i = len(s)-3
        while i>0:
            memo[i] = isValid(s[i:i+2])*memo[i+2]+isValid(s[i])*memo[i+1]
            i-=1
        memo[0] = isValid(s[0])*memo[1]
        
        return memo[0]
    
    ```

  + DP save space: we are only using `memo[i+1], memo[i+2]`

    ```python
    def numDecodings(s):
        if len(s)==1: return isValid(s)
        l = len(s)-1
        last2 = isValid(s[l])
        last1 = isValid(s[l-1:])+isValid(s[l])*isValid(s[l-1])
        if len(s)==2: return last1
        i = len(s)-3
        while i>=0:
            cur = isValid(s[i:i+2])*last2+isValid(s[i])*last1
            last1, last2=cur, last1
            i-=1
        return cur
    
    def isValid(s):
        if len(s)==0: return 0
        if len(s)==1:
            return 0 if s=="0" else 1
        if len(s)==2:
            if int(s)>=10 and int(s)<=26: return 1
            else: return 0
            
    ```

    

### 121  Best Time to Buy and Sell Stock

Say you have an array for which the *i*th element is the price of a given stock on day *i*.

If you were only permitted to complete at most one transaction (i.e., buy one and sell one share of the stock), design an algorithm to find the maximum profit.

Note that you cannot sell a stock before you buy one.



+ Example

  + ```
    Input: [7,1,5,3,6,4]
    Output: 5
    Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
                 Not 7-1 = 6, as selling price needs to be larger than buying price.
    ```

  + ```
    Input: [7,6,4,3,1]
    Output: 0
    Explanation: In this case, no transaction is done, i.e. max profit = 0.
    ```

+ Analysis

  + Find index i and j where j>i and `prices[j]-prices[i]` is maxed.

  + Brutal force: `for i in range(len(prices))`test each j

  + Note that when we repeat computation when we are scanning for `j`'s

  + Recursion:

    ```python
    maxProfit(prices, i):
        with_i = max(prices)-i
        without_i = maxProfit(prices, i+1)
        return max(with_i, without_i)
    ```

  + Stack:

    + push if stack empty
    + pop all if `price<stack[0]`
    + pop top and push if `price>stack[top]`
    + Keep a record of max `stack[top]-stack[0]`

  + We are only using top and bottom of stack. Substitute with 2 integer variables instead

    ```python
    if len(prices)<1: return 0
    max_profit = 0
    bottom = prices[0]
    top = bottom
    for price in prices:
        if price<bottom:
            bottom=price
            top=bottom
        elif price>top:
            top = price
            max_profit = max(top-bottom, max_profit)
    return max_profit
    ```

+ Tips:

  + when have order-constrained extrema problem, try stack first. If only use bottom and top of stack, substitute with 2 integer variables..



### 139 word break

Given a **non-empty** string *s* and a dictionary *wordDict* containing a list of **non-empty** words, determine if *s* can be segmented into a space-separated sequence of one or more dictionary words.

**Note:**

- The same word in the dictionary may be reused multiple times in the segmentation.
- You may assume the dictionary does not contain duplicate words.



+ Example

  + ```
    Input: s = "leetcode", wordDict = ["leet", "code"]
    Output: true
    Explanation: Return true because "leetcode" can be segmented as "leet code".
    ```

  + ```
    Input: s = "applepenapple", wordDict = ["apple", "pen"]
    Output: true
    Explanation: Return true because "applepenapple" can be segmented as "apple pen apple".
                 Note that you are allowed to reuse a dictionary word.
    ```

  + ```
    Input: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
    Output: false
    ```

+ Analysis

  + wordDict is a list. We need to check whether a word is in dict fast. So first convert it into a set `wordSet`, and find a `maxWordLen` and `minWordLen` for future use.

  + Decoding problem similar to # 91 and # 70

  + base case:

    + if `s in wordSet: return True`
    + if `len(s)<minWordLen: return False`

  + Recursion

    ```python
    start = 0
    wordSet
    s
    hasBreak(start):
        if baseCase: return baseCase
        for wordLen in range(minWordLen, maxWordLen+1):
            if s[:wordLen] in wordSet && hasBreak(start+wordLen):
                return True
        return False
    
    ```

  + Recursion with memory: we can store a truth array indexed by start:

    ```python
    memo[start] = [-1 for _ in len(s)]
    
    hasBreak(start):
        if memo[start]!=-1: return memo[start]
        if baseCase: return baseCase
        for wordLen in range(minWordLen, maxWordLen+1):
            if s[:wordLen] in wordSet && hasBreak(start+wordLen):
                memo[start]=1
                return True
        memo[start]=0
        return False
    
    ```

  + DP with memory: goes in same direction 

    + direction of expansion: wordLen
    + direction of traverse: left to right of the string

    ```python
    memo[start] = [False for _ in range(len(s))] # can start from index
    for wordLen in range(minWordLen, maxWordLen+1):
        if s[:wordLen] in wordSet:
            memo[wordLen]=True
    
    for wordLen in range(minWordLen, maxWordLen+1):
        start=minWordLen
        while start<len(s)-minWordLen:
            if s[start:wordLen] in wordSet and memo[start]: 
                memo[start+wordLen]=True
            start+=wordLen
    return memo[-1]
    ```

  + DP save space:

    + note that we only use `memo[start: start+maxWordLen]` in each step. We only need a constant size array of size `maxWordLen`

    + To do this we need to do traverse first then expand wordLen

    + Each index offsets the memo array by 1

    + We can skip all memo=0 index for next iteration

    + if previous memo is 1, then copy that 1

    + eg: applepenapple, ["a","apple","pen","app"], maxWordLen=5, minWordLen=1

      |      | a    | p    | p    | l    | e    | p    | e    | n    | a    | p    | p    | l    | e    | a    |      |
      | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
      | 0    | 1    | 1    | 0    | 1    | 0    | 1    |      |      |      |      |      |      |      |      |      |
      | 1    |      | 1    | 0    | 1    | 0    | 1    | 0    |      |      |      |      |      |      |      |      |
      | 2    |      |      |      | 1    | 0    | 1    | 0    | 0    | 0    |      |      |      |      |      |      |
      | 3    |      |      |      |      |      | 1    | 0    | 0    | 1    | 0    | 0    |      |      |      |      |
      | 4    |      |      |      |      |      |      |      |      | 1    | 1    | 0    | 1    | 0    | 1    |      |
      | 5    |      |      |      |      |      |      |      |      |      | 1    | 0    | 1    | 0    | 1    | 0    |
      | 6    |      |      |      |      |      |      |      |      |      |      |      | 1    | 0    | 1    | 0    |
      | 7    |      |      |      |      |      |      |      |      |      |      |      |      |      | 1    | 1    |

    ```python
    memo = [False for _ in range(maxWordLen+1)]
    memo[0] = True
    start = 0
    stringLen=len(s)
    for wordLen in range(minWordLen, maxWordLen+1):
        if s[:wordLen] in wordSet:
            memo[wordLen]=True
            if start==0:
                start=wordLen
    offset=start
    while start<=stringLen-minWordLen:
        memo=memo[offset:]+[False for _ in range(offset)]
        copiedLength = maxWordLen-offset
        offset=0
        wordLen=minWordLen
        while wordLen<=maxWordLen and start+wordLen<=stringLen:
            if memo[wordLen] and offset==0: 
                	newStart=start+wordLen
                    wordLen = copiedLength
        	if s[start:start+wordLen] in wordSet:
            	memo[wordLen]=True
            wordLen+=1
       	offset=newStart-start
        start=newStart
    return memo[-1]
    ```

  + The above acceleration is making things more complicated. Use simple DP instead:

    ```python
    def wordBreak(self, s, wordDict):
        memo=[True]
        words=set(wordDict)
        for i in range(1, len(s)+1):
            memo += any(memo[j] and s[j:i] in words for j in range(i))
        return memo[-1]
    ```

    

### 152 Maximum Product Subarray

Given an integer array `nums`, find the **contiguous** subarray within an array (containing at least one number) which has the largest product.

+ Example

  + ```
    Input: [2,3,-2,4]
    Output: 6
    Explanation: [2,3] has the largest product 6.
    ```

  + ```
    Input: [-2,0,-1]
    Output: 0
    Explanation: The result cannot be 2, because [-2,-1] is not a subarray.
    ```

+ Analysis

  + base case: 

    + `len(nums)==0` out 0 
    + `len(nums)==1` out nums[0]
    + `len(nums)==2`out `max(nums[0], nums[0]*nums[1], nums[1])`

  + DP:

    + direction of expansion: `len(subarray)+=1`
    + direction of traversal: `i+=1`
    + memory: `memo[i][j]=prod(start,end)`
    + time and space: $O(n^2)$

    ```python
            length = len(nums)
            memo = [[0 for _ in range(length)] for _ in range(length)]
            maxProd = nums[0]
            for i in range(length):  # len(subarray)==1
                memo[i][i] = nums[i]
                maxProd = max(nums[i], maxProd)
            for subLen in range(2, length + 1):
                for start in range(0,length - subLen+1):
                    end = start + subLen - 1
                    memo[start][end] = nums[end] * memo[start][end - 1]
                    maxProd = max(memo[start][end], maxProd)
            return maxProd
    ```

  + DP to save space: first iterate through the array, then iterate over length

    ```python
    length = len(nums)
    maxProd = nums[0]
    for i in range(length):
        product = 1
        for j in range(i,length):
            product*=nums[j]
            result = max(result, product)
    return result
    ```

    

  + Improvement to reduce time: $O(n)$, following conditions for `num[i]`

    + positive: -> `prevMax*num[i]`
    + negative: ->`max(prevMin*num[i], num[i])`
    + zero: -> `num[i]`
    + Then we only need to traverse once, and keep a record of `curMin, curMax`

    ```python
    length = len(nums)
    maxProd = nums[0]
    curMax = nums[0]
    curMin = nums[0]
    for i in range(1,length):
        prevMax = curMax
        prevMin = curMin
        curMax = max(prevMax*nums[i], prevMin*nums[i], nums[i])
        curMin = min(prevMax*nums[i], prevMin*nums[i], nums[i])
        maxProd = max(curMax, maxProd)
    return maxProd
    
    ```

    

+ Tips:

  + For finding contiguous subarray, it usually saves space to first iterate over array, then iterate over subarray size. 
  + After find the solution, try to eliminate the iterations over subarray by memorizing results on the go during the first iteration



### 198 House Robber

max sum subarray with no adjacent elements. All elements between `[0,400]`

+ Examples:

  + ```
    Input: nums = [1,2,3,1]
    Output: 4
    Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
                 Total amount you can rob = 1 + 3 = 4.
    ```

  + ```
    Input: nums = [2,7,9,3,1]
    Output: 12
    Explanation: Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
                 Total amount you can rob = 2 + 9 + 1 = 12.
    ```

  + `[2,1,1,2]=> 4`: note: can skip more than one house

+ Analysis

  ```python
          n = len(nums)
          if n==0:
              return 0
          if n==1:
              return nums[0]
          last2 = 0
          last1 = nums[0]
          for i in range(1,n):
              curMax = max(nums[i]+last2, last1) #legal robs up to current point
              last2, last1  = last1, curMax
          return curMax
  ```

  

  

### 213 House Robber II

max sum subarray with no adjacent elements. Subarray is arranged in a circle (first and last element cannot appear together)

+ Example:

  + ```
    Input: nums = [2,3,2]
    Output: 3
    Explanation: You cannot rob house 1 (money = 2) and then rob house 3 (money = 2), because they are adjacent houses.
    ```

  + ```
    Input: nums = [1,2,3,1]
    Output: 4
    Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
    Total amount you can rob = 1 + 3 = 4.
    ```

  + ```
    Input: nums = [0]
    Output: 0
    ```



+ Extend from simple house robber:

  + take out the last element, then the remaining array is a problem of simple house robber
  + we need to remember if `nums[0]`is taken, if it is taken, then we cannot take the last element. If not, we can. 
  + Two passes of simpleHouseRobber: 

  ```python
      def rob(self, nums: List[int]) -> int:
          n = len(nums)
          if n<=0:
              return 0
          if n==1:
              return nums[0]
          return max(self.simpleHouseRobber(nums[:-1]), self.simpleHouseRobber(nums[1:]))
      def simpleHouseRobber(self, nums):
          n = len(nums)
          if n<=0:
              return 0
          if n==1:
              return nums[0]
          last2 = 0
          last1 = nums[0]
          for i in range(1,n):
              curMax = max(nums[i]+last2, last1) #legal robs up to current point
              last2, last1  = last1, curMax
          return curMax
  ```

  + Optimize time: try to do them in single pass

  ```python
      def rob(self, nums: List[int]) -> int:
          n = len(nums)
          if n <= 0:
              return 0
          if n == 1:
              return nums[0]
          if n == 2:
              return max(nums[0], nums[1])
          last2 = nums[0]
          last1 = max(nums[1],nums[0])
          last2Late= 0
          last1Late = nums[1]
          for i in range(2, n - 1):
              curMax = max(nums[i] + last2, last1)
              last2, last1 = last1, curMax
              curMaxLate = max(nums[i] + last2Late, last1Late)
              last2Late, last1Late = last1Late, curMaxLate
          curMaxLate = max(nums[-1] + last2Late, last1Late)
          return max(last1,curMaxLate)
  ```


  + The above two solution are both $O(n)$ , the second solution is slightly faster, but first solution is more clear and concise, and saves space.

+ 

### 303 Range Sum Query - Immutable

Given an integer array `nums`, find the sum of the elements between indices `i` and `j` `(i ≤ j)`, inclusive.

Implement the `NumArray` class:

- `NumArray(int[] nums)` Initializes the object with the integer array `nums`.
- `int sumRange(int i, int j)` Return the sum of the elements of the `nums` array in the range `[i, j]` inclusive (i.e., `sum(nums[i], nums[i + 1], ... , nums[j])`)

+ Example

  + ```
    Input
    ["NumArray", "sumRange", "sumRange", "sumRange"]
    [[[-2, 0, 3, -5, 2, -1]], [0, 2], [2, 5], [0, 5]]
    Output
    [null, 1, -1, -3]
    
    Explanation
    NumArray numArray = new NumArray([-2, 0, 3, -5, 2, -1]);
    numArray.sumRange(0, 2); // return 1 ((-2) + 0 + 3)
    numArray.sumRange(2, 5); // return -1 (3 + (-5) + 2 + (-1)) 
    numArray.sumRange(0, 5); // return -3 ((-2) + 0 + 3 + (-5) + 2 + (-1))
    ```

  + brutal: simply sum

  + acceleration: use 2D array to memorize all sums of start-end pair

  + More acceleration (assume read heavy): use 1D array to memorize leftSum (not inclusive) in all indexes. and sum in each interval is the `leftSum[right+1]-leftSum[left]`

  ```python
  class Solution:
      def __init__(self, nums):
          self.leftSums = [0]
          curSum=0
          for num in nums:
              curSum+=num
              self.leftSums+=[curSum]
      def sumRange(self, i: int, j: int) -> int:
          return self.leftSums[j+1]-self.leftSums[i]
  ```

  

### 309 Best Time to Buy and Sell Stock with Cooldown

Say you have an array for which the *i*th element is the price of a given stock **on day *i*.**

Design an algorithm to find the maximum profit. You may complete **as many transactions as you like** (ie, buy one and sell one share of the stock multiple times) with the following restrictions:

You may **not engage in multiple transactions** at the same time (ie, you must sell the stock before you buy again).

**After you sell your stock, you cannot buy stock on next day. (ie, cooldown 1 day)**

+ Example

  + ```
    Input: [1,2,3,0,2]
    Output: 3 
    Explanation: transactions = [buy, sell, cooldown, buy, sell]
    ```

+ Answer: State Machine for all buy and sell stock problems
  [reference](https://www.thealgorists.com/Algo/DynamicProgramming/StateMachine/StockTradingWithCooldown)

  + Three states:

    

    ```mermaid
    graph LR
    	hasStock --1.sell_addProfit--> justSold
    	justSold --2.doNothing--> cooldownDone
    	cooldownDone --3.doNothing--> cooldownDone
    	cooldownDone --4.buy_reduceProfit--> hasStock
    	hasStock --5.doNothing--> hasStock
    ```

  + DP relations: max profit on a day if we choose certain operation

    ```python
    # max(3,2)
    coodownDone[day] = max(cooldownDone[day-1],justSold[day-1])
    # max(4,5)
    hasStock[day] = max(cooldownDone[day-1]-prices[day], hasStock[day-1])
    # 1
    justSold[day] = hasStock[day-1]+prices[day]
    ```

  + Base cases

    ```python
    hasStock[0]=0
    cooldownDone[0]=0
    justSold[0]=Integer.MIN_VALUE #selling on first day is not possible, use min_value for a maximization problem
    ```

  + Return value: we need to have no stock in hand on the last day

    ```python
    return max(cooldownDone[-1], justSold[-1])
    ```

  + DP

    ```python
    n = len(prices)
    if n<=1: return 0
    cooldownDone = [0]
    hasStock = = [-prices[0]]
    justSold = = [-sys.maxint-1]
    
    for day in range(1,n):
        coodownDone[day] = max(cooldownDone[day-1],justSold[day-1])
        hasStock[day] = max(cooldownDone[day-1]-prices[day], hasStock[day-1])
        justSold[day] = hasStock[day-1]+prices[day]
    return max(cooldownDone[-1], justSold[-1])
    
    ```

    ```java
  class Solution {
        public int maxProfit(int[] prices) {
            int n = prices.length;
            if (n==0)
                return 0;
            int[][] dp = new int[n][3];
            dp[0][0] = -prices[0];
            dp[0][1] = Integer.MIN_VALUE;
            for(int i=1; i<n; i++){
                dp[i][0] = Math.max(dp[i-1][0], dp[i-1][2] - prices[i]);
                dp[i][1] = dp[i-1][0] + prices[i];
                dp[i][2] = Math.max(dp[i-1][2], dp[i-1][1]);           
            }
            
            return Math.max(Math.max(dp[n-1][1], dp[n-1][2]),0);
      }
    }
  ```
  
    
  
  + DP save space: we only use `[day-1]` values in each iteration
  
    ```python
    n = len(prices)
    if n<=1: return 0
    lastCooldownDone = 0
    lastHasStock = = 0
    lastJustSold = = -sys.maxint-1
    
    for day in range(1,n):
        coodownDone= max(lastCooldownDone,lastJustSold)
        hasStock = max(lastCooldownDone-prices[day], lastHasStock)
        justSold = lastHasStock+prices[day]
        lastCooldownDone, lastHasStock, lastJustSold = cooldownDone, hasStock, justSold
    return max(lastCooldownDone, lastJustSold)
    ```
  
  + If want to record the optimal path
  
    ```python
    purchaseDays =[] # purchaseDays[i] = last Purchase day that would maximize profit fo day i
    saleDays =[] #saleDays[i] = last sale day that would maximize profit fo day i
    tempPurchaseDays =[] # tempPurchaseDay[i] gives the purchase day which maximizes hasStock state for day i
    for day in range(1,n):
        cooldownDone= max(lastCooldownDone,lastJustSold)
        hasStock = max(lastCooldownDone-prices[day], lastHasStock)
        justSold = lastHasStock+prices[day]
        
        if justSold>cooldownDone: #sell
            saleDays+=[day]
            purchaseDays+=[tempPurchaseDays[day-1]]
        else: # did nothing
            saleDays+= [saleDays[day-1]]
            purchaseDays+=[tempPurchaseDays[day-1]]
        if hasStock > lastHasStock: #buy
            tempPurchaseDays+=[day]
        else: # did nothing
            tempPurchaseDays+=[tempPurchaseDays[day-1]]
        
        lastCooldownDone, lastHasStock, lastJustSold = cooldownDone, hasStock, justSold
    ```



### 322 Coin Change

You are given coins of different denominations and a total amount of money *amount*. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return `-1`. You may assume that you have an infinite number of each kind of coin.

+ Example

  + ```
    Input: coins = [1,2,5], amount = 11
    Output: 3
    Explanation: 11 = 5 + 5 + 1
    ```

  + ```
    Input: coins = [2], amount = 3
    Output: -1
    ```

  + ```
    Input: coins = [1], amount = 0
    Output: 0
    ```

  + ```
    Input: coins = [1], amount = 1
    Output: 1
    ```

  + ```
    Input: coins = [1], amount = 2
    Output: 2
    ```

+ Analysis

  + Base case:

    + `len(coins)==1`: `amount%coins[0]==0? amount/coins[0] else -1`

  + Recursion:

    + Assume we have value of coins ordered from small to large

    + Then 

      ```python
      coinChange(coins, amount):
          if len(coins)==1:
              if amount%coins[0]==0:
                  return amount/couns
              else:
                  return -1
          curValue=coins[-1]
          curnum = int(amount/curValue)
          res = sys.maxint
          found=False
          while curnum>=0:
              prev = coinChange(coins[:-1],amount-curnum*curValue)
              if prev>-1:
              	res = min(res, curnum+prev)
                  found=True
          if not found: return -1
          return res
              
      ```

  + Recursion with Memorization: `memo[len(coins)][amount]`

  + DP: from 0 to amount, each step, `memo[i]`is given by adding 1 to `memo[i-value]`for each value of coins

    | Value | 0    | 1    | 2    | 3    | 4    | 5    |
    | ----- | ---- | ---- | ---- | ---- | ---- | ---- |
    | 1     | 0    | 1    | 2    | 3    | 4    | 5    |
    | 2     | 0    | 1    | 1    | 2    | 2    | 3    |
    | 5     | 0    | 1    | 1    | 2    | 2    | 1    |
    | min   | 0    | 1    | 1    | 2    | 2    | 1    |

    ```python
       import sys
    
    class Solution:
        def coinChange(self, coins: List[int], amount: int) -> int:
            if not coins or not amount: return 0
            dp = [0]* (amount+1)
            for i in range(amount+1):
                if i%coins[0]==0:
                    dp[i] = i//coins[0]
                else:
                    dp[i] = sys.maxsize
                    
            for i in range(1,len(coins)):
                for j in range(1,amount+1):
                    if j>=coins[i]:
                        dp[j] = min(dp[j-coins[i]]+1,dp[j])
            return dp[-1] if dp[-1]!=sys.maxsize else -1
    ```

+ 

### 338 Counting Bits

Given a non negative integer number **num**. For every numbers **i** in the range **0 ≤ i ≤ num** calculate the number of 1's in their binary representation and return them as an array.

+ Examples

  + ```
    Input: 2
    Output: [0,1,1]
    ```

  + ```
    Input: 5
    Output: [0,1,1,2,1,2]
    ```

+ Analysis

  + | 0    | 0    | out  | $2^n+k$: n | k    |
    | ---- | ---- | ---- | ---------- | ---- |
    | 1    | 01   | 1    | 0          | 1    |
    | 2    | 10   | 1    | 1          | 0    |
    | 3    | 11   | 2    | 1          | 1    |
    | 4    | 100  | 1    | 2          | 0    |
    | 5    | 101  | 2    | 2          | 1    |
    | 6    | 110  | 2    | 2          | 2    |
    | 7    | 111  | 3    | 2          | 3    |
    | 8    | 1000 | 1    | 3          | 0    |
    | 9    | 1001 | 2    | 3          | 1    |

    + n: add 1
    + k: add the value given by `memo[k]`

  + Now we need to find the fastest way to calculate n and k. Note that n always increases, we can just use the previous n and add 1 if needed. And we can store the exponential directly so we don't need to calculate 2^n every time

  ```python
  countBits(num):
      array=[0]
      exponential=1 #2^n
      for i in range(1,num+1):
          if 2*exponential==i:
              exponential*=2
              array+=[1]
          else:
              array+=[1+array[i-exponential]]
      return array
  ```

  + We actually don't need to calculate n, we shifts bits to left when multiply a number by 2 (adding 0 on the right). Then we can just take `memo[i//2]`

  ```python
  memo=[0]
  for i in range(1,num+1):
      memo+=[memo[i//2]+i%2]
  return memo
  ```

+ Tips:

  + `count1s[n]=count1s[n//2]+n%2`



### 337 Combination Sum IV

Given an integer array with all positive numbers and no duplicates, find the number of possible combinations that add up to a positive integer target.

+ Example: 

  + ```
    nums = [1, 2, 3]
    target = 4
    
    The possible combination ways are:
    (1, 1, 1, 1)
    (1, 1, 2)
    (1, 2, 1)
    (1, 3)
    (2, 1, 1)
    (2, 2)
    (3, 1)
    
    Note that different sequences are counted as different combinations.
    
    Therefore the output is 7.
    ```

+ Analysis

  + This is similar to the coin change problem, but only need to count the ways

  + | Target     | 0    | 1    | 2    | 3    | 4    | 5    |
    | ---------- | ---- | ---- | ---- | ---- | ---- | ---- |
    | 1          | 0    | 1    | 1    | 2    | 4    | 7    |
    | 2          |      |      | 1    | 1    | 2    | 4    |
    | 3          |      |      | 0    | 1    | 1    | 2    |
    | ways (sum) | 1    | 1    | 2    | 4    | 7    | 13   |

  + ```python
    if target==0: return 0
    memo=[1]
    for i in range(1,target+1):
        curWays=0
        for num in nums:
            if num<=i:
            	curWays+=memo[i-num]
        memo+=[curWays]
    return memo[-1]
    ```



### 416 Partition Equal Subset Sum

Given a **non-empty** array `nums` containing **only positive integers**, find if the array can be partitioned into two subsets such that the sum of elements in both subsets is equal.

+ Example

  + ```
    Input: nums = [1,5,11,5]
    Output: true
    Explanation: The array can be partitioned as [1, 5, 5] and [11].
    ```

  + ```
    Input: nums = [1,2,3,5]
    Output: false
    Explanation: The array cannot be partitioned into equal sum subsets.
    ```

+ Analysis

  + The sum of each subsets must be totalsum/2

  + Then the problem is equivalent to find a subset with sum of totalsum/2

  + Recursion: $O(2^n)$  

    ```python
    nums
    canSum(start, target):
        curnum=nums[start]
        if curnum==0:
            return True
        if curnum>target:
            return False
        else:
            # take or not take
            return canSum(nums[start+1:], target) and canSum(nums[start+1:], target-curnum)
    ```

  + Recursion with memorization: `memo[len(nums)][target]` 

  + DP similar to last problem:
  + | Target    | 0     | 1    | 2    | 3    | 4    |
    | --------- | ----- | ---- | ---- | ---- | ---- |
    | 1         | 0     | 1    | 0    | 0    | 1    |
    | 3         |       |      | 0    | 1    | 1    |
    | 4         |       |      | 0    | 0    | 1    |
    | Remaining | 1,3,4 | 3,4  |      | 1,4  | 4    |
    | canSum    | 1     | 1    | 0    | 1    | 1    |

```python
    def canPartition(self, nums) -> int:
        if len(nums)<=1:
            return False
        if sum(nums)%2!=0:
            return False
        return self.canSum(nums, sum(nums)//2)

    def canSum(self,nums, target):
        if target == 0: return True
        memo = [True]
        remaining = [nums]
        for i in range(1, target + 1):
            for num in nums:
                if num <= i:
                    if memo[i - num] and num in remaining[i - num]:
                        memo += [True]
                        copy = [i for i in remaining[i - num]]
                        copy.remove(num)
                        remaining += [copy]
                        break
            else:
                memo += [False]
                remaining += [[-1]]

        return memo[-1]

```


​    

+ Tips: general method for coin change/ subset sum problem:
 + | Nums \ Target                                                | 0    | 1    | 2    | 3    | 4    | Target |
    | ------------------------------------------------------------ | ---- | ---- | ---- | ---- | ---- | ------ |
    | 1                                                            |      |      |      |      |      |        |
    | 2                                                            |      |      |      |      |      |        |
    | 3                                                            |      |      |      |      |      |        |
    | Remaining nums ( use if without replacement)                 |      |      |      |      |      |        |
    | ways (sum)<br /> canSum (if subproblem gives true)<br /> min/max(if maximization or minimization problem) |      |      |      |      |      |        |
    
    + ```python
      def f(self,nums, target):
          if target == 0: return True
          memo = [True]
          remaining = [nums]
          for i in range(1, target + 1):
              for num in nums:
                  if num <= i:
                      if memo[i - num] and num in remaining[i - num]:
                          memo += [True]
                          copy = [i for i in remaining[i - num]]
                          copy.remove(num)
                          remaining += [copy]
                          break
              else:
                  memo += [False]
                  remaining += [[-1]]
      
          return memo[-1]
      ```



### 1143. Longest Common Subsequence

Medium

246930Add to ListShare

Given two strings `text1` and `text2`, return the length of their longest common subsequence.

A *subsequence* of a string is a new string generated from the original string with some characters(can be none) deleted without changing the relative order of the remaining characters. (eg, "ace" is a subsequence of "abcde" while "aec" is not). A *common subsequence* of two strings is a subsequence that is common to both strings.

 

If there is no common subsequence, return 0.

 

**Example 1:**

```
Input: text1 = "abcde", text2 = "ace" 
Output: 3  
Explanation: The longest common subsequence is "ace" and its length is 3.
```

**Example 2:**

```
Input: text1 = "abc", text2 = "abc"
Output: 3
Explanation: The longest common subsequence is "abc" and its length is 3.
```

**Example 3:**

```
Input: text1 = "abc", text2 = "def"
Output: 0
Explanation: There is no such common subsequence, so the result is 0.
```

+ Go over one string, for each index:
  + find corresponding index in another string (starting from current index)
    + if not found starting from current index: find before current index
    + If not found overall, then do nothing
  + maintain a curIdx found in the previous step
  + increment length if curIdx increasing, change to 0 if decreasing



+ Solution:
+ Each cell represents one subproblem. For example, the below cell represents the subproblem `lcs("attag", "gtgatcg")`.



![Cell where first letter is same, showing +1 into new cell](https://leetcode.com/problems/longest-common-subsequence/Figures/1143/bottom_up_same_letter.png)

subproblem that removes the first letter off the first word, and then the subproblem that removes the first letter off the second word. In the grid, these are subproblems immediately right and below.

![Cell where first letter is same, showing +1 into new cell](https://leetcode.com/problems/longest-common-subsequence/Figures/1143/bottom_up_different_letter.png)

```python
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        
        n1, n2 = len(text1), len(text2)
        dp = [[0]*(n2+1) for _ in range(n1+1)]
        for i in range(1, n1+1):
            for j in range(1, n2+1):
                if text1[i-1]==text2[j-1]:
                    dp[i][j] = dp[i-1][j-1] + 1
                else:
                    dp[i][j] = max(dp[i][j-1], dp[i-1][j])
        
        return dp[-1][-1]
```

```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int n1 = text1.length();
        int n2 = text2.length();
        int[][] dp = new int[n1+1][n2+1];
        
        for (int i = 1; i < n1 + 1; i++){
            for (int j = 1; j < n2 + 1; j++){
                if (text1.charAt(i-1) == text2.charAt(j-1)){
                    dp[i][j] = dp[i-1][j-1] + 1;
                } else{
                    dp[i][j] = Math.max(dp[i-1][j], dp[i][j-1]);
                }
            }
        }
        
        return dp[n1][n2];
        
    }
}
```

```java
class Solution {
    public String shortestCommonSupersequence(String str1, String str2) {
        int n1 = str1.length();
        int n2 = str2.length();
        int[][] dp = new int[n1+1][n2+1];
        for (int i = 1; i <= n1; i++){
            for (int j = 1; j <= n2; j++){
                if (str1.charAt(i-1) == str2.charAt(j-1)){
                    dp[i][j] = dp[i-1][j-1] + 1;
                }else{
                    dp[i][j] = Math.max(dp[i-1][j], dp[i][j-1]);
                }
            }
        }
        
        int i = n1;
        int j = n2;
        StringBuilder sb = new StringBuilder();
        while (i>0 || j>0){
            if (i == 0){
                sb.append(str2.charAt(--j));
            }else if (j == 0){
                sb.append(str1.charAt(--i));
            }else if (str1.charAt(i-1) == str2.charAt(j-1)){
                sb.append(str1.charAt(--i));
                j--;
            }else if (dp[i][j] == dp[i-1][j]){
                sb.append(str1.charAt(--i));
            }else if (dp[i][j] == dp[i][j-1]){
                sb.append(str2.charAt(--j));
            }
            
        }
        StringBuilder reversed = sb.reverse();
        return reversed.toString();
            
    }
}
```



### 1092. Shortest Common Supersequence

Hard

Given two strings `str1` and `str2`, return the shortest string that has both `str1` and `str2` as subsequences. If multiple answers exist, you may return any of them.

*(A string S is a subsequence of string T if deleting some number of characters from T (possibly 0, and the characters are chosen anywhere from T) results in the string S.)*

 

**Example 1:**

```
Input: str1 = "abac", str2 = "cab"
Output: "cabac"
Explanation: 
str1 = "abac" is a subsequence of "cabac" because we can delete the first "c".
str2 = "cab" is a subsequence of "cabac" because we can delete the last "ac".
The answer provided is the shortest such string that satisfies these properties.
```

+ With LCS dp:
  ![image-20210130150157208](/home/arkyyang/files/notes/notes/attachments/image-20210130150157208.png)







```python
    len1 = len(str1)
    len2 = len(str2)
    dp_str = ['']*(len2+1)
    for j in range(len2+1):
        dp_str[j] = str2[:j]


    for i in range(1, len1+1):
        prev = list(dp_str)
        for j in range(1, len2+1):
            cur1 = str1[i-1]
            cur2 = str2[j-1]
            dp_str[0] = str1[:i]

            if cur1 == cur2:
                dp_str[j] = prev[j-1]+cur1
            elif len(prev[j]) > len(dp_str[j-1]):
                dp_str[j] = dp_str[j-1]+cur2
            else:
                dp_str[j] = prev[j]+cur1  

    return dp_str[-1]
```

```python
import sys
class Solution:
    def shortestCommonSupersequence(self, str1: str, str2: str) -> str:
        n1, n2 = len(str1), len(str2)
        dp = [[0]*(n2+1) for _ in range(n1+1)]
        for i in range(1, n1+1):
            for j in range(1, n2+1):
                if str1[i-1] == str2[j-1]:
                    dp[i][j] = dp[i-1][j-1] + 1
                else:
                    dp[i][j] = max(dp[i-1][j], dp[i][j-1])
        
        i, j = n1, n2
        res = []
        while i or j:
            if i == 0:
                res.append(str2[j-1])
                j -= 1
            elif j == 0:
                res.append(str1[i-1])
                i -= 1
            elif str1[i-1] == str2[j-1]:
                res.append(str1[i-1])
                i-=1
                j-=1
            elif dp[i][j] == dp[i-1][j]:
                res.append(str1[i-1])
                i -= 1
            elif dp[i][j] == dp[i][j-1]:
                res.append(str2[j-1])
                j -= 1
                
        res.reverse()
        return ''.join(res)
```



### 174. Dungeon Game

Hard

The demons had captured the princess (**P**) and imprisoned her in the bottom-right corner of a dungeon. The dungeon consists of M x N rooms laid out in a 2D grid. Our valiant knight (**K**) was initially positioned in the top-left room and must fight his way through the dungeon to rescue the princess.

The knight has an initial health point represented by a positive integer. If at any point his health point drops to 0 or below, he dies immediately.

Some of the rooms are guarded by demons, so the knight loses health (*negative* integers) upon entering these rooms; other rooms are either empty (*0's*) or contain magic orbs that increase the knight's health (*positive* integers).

In order to reach the princess as quickly as possible, the knight decides to move only rightward or downward in each step.

 

**Write a function to determine the knight's minimum initial health so that he is able to rescue the princess.**

For example, given the dungeon below, the initial health of the knight must be at least **7** if he follows the optimal path `RIGHT-> RIGHT -> DOWN -> DOWN`.

| -2 (K) | -3   | 3      |
| ------ | ---- | ------ |
| -5     | -10  | 1      |
| 10     | 30   | -5 (P) |

 

+ Dijstra's algorithm for weighted shortest path

+ Find max min-health path, return min-health+1



```python
class Solution(object):
    def calculateMinimumHP(self, dungeon):
        """
        :type dungeon: List[List[int]]
        :rtype: int
        """
        if not dungeon: return 1
        rows, cols = len(dungeon), len(dungeon[0])
        dp = [[float('inf')]*cols for _ in range(rows)]

        
        
        def get_min_health(curCell, ni, nj):
            if ni>=rows or nj >=cols:
                return float('inf')
            nextCell = dp[ni][nj]
            return max(1, nextCell-curCell)
        
        for i in reversed(range(rows)):
            for j in reversed(range(cols)):
                cur = dungeon[i][j]
                right_health = get_min_health(cur, i, j+1)
                down_health = get_min_health(cur, i+1, j)
                next_health = min(right_health, down_health)
                if next_health != float('inf'):
                    min_health = next_health
                else: 
                    min_health = 1 if cur>=0 else (1-cur)
                
                dp[i][j] = min_health
                
        return dp[0][0]
                
```





### 63. Unique Paths II

Medium

A robot is located at the top-left corner of a `m x n` grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

Now consider if some obstacles are added to the grids. How many unique paths would there be?

An obstacle and space is marked as `1` and `0` respectively in the grid.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/11/04/robot1.jpg)

```
Input: obstacleGrid = [[0,0,0],[0,1,0],[0,0,0]]
Output: 2
Explanation: There is one obstacle in the middle of the 3x3 grid above.
There are two ways to reach the bottom-right corner:
1. Right -> Right -> Down -> Down
2. Down -> Down -> Right -> Right
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/11/04/robot2.jpg)

```
Input: obstacleGrid = [[0,1],[0,0]]
Output: 1
```

 ```python
class Solution(object):
    def uniquePathsWithObstacles(self, obstacleGrid):
        """
        :type obstacleGrid: List[List[int]]
        :rtype: int
        """
        if not obstacleGrid: return 0
        rows, cols = len(obstacleGrid), len(obstacleGrid[0])
        
        dp = [[0]*cols for _ in range(rows)]

        for i in range(rows):
            if obstacleGrid[i][0]!=1:
                dp[i][0] = 1
            else:
                break
        
        for j in range(cols):
            if obstacleGrid[0][j]!=1:
                dp[0][j] = 1
            else:
                break
        
        for i in range(1,rows):
            for j in range(1,cols):
                if obstacleGrid[i][j]==1:
                    dp[i][j]=0
                else:
                    dp[i][j] = dp[i-1][j]+dp[i][j-1]
        
        return dp[-1][-1]
 ```



```python
        if not obstacleGrid or obstacleGrid[0][0]==1 : return 0
        rows, cols = len(obstacleGrid), len(obstacleGrid[0])
        
        dp = [0]*cols

        
        for j in range(cols):
            if obstacleGrid[0][j]!=1:
                dp[j] = 1
            else:
                break
        
        for i in range(1,rows):
            if obstacleGrid[i][0]==1 or dp[0]==0:
                dp[0]=0
            else:
                dp[0]=1
            for j in range(1,cols):
                if obstacleGrid[i][j]==1:
                    dp[j]=0
                else:
                    dp[j] = dp[j]+dp[j-1]
        
        return dp[-1]
                
```



### 64. Minimum Path Sum

Medium

Given a `m x n` `grid` filled with non-negative numbers, find a path from top left to bottom right, which minimizes the sum of all numbers along its path.

**Note:** You can only move either down or right at any point in time.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/11/05/minpath.jpg)

```
Input: grid = [[1,3,1],[1,5,1],[4,2,1]]
Output: 7
Explanation: Because the path 1 → 3 → 1 → 1 → 1 minimizes the sum.
```

**Example 2:**

```
Input: grid = [[1,2,3],[4,5,6]]
Output: 12
```

 

```python
class Solution:
    def minPathSum(self, grid: List[List[int]]) -> int:
        if not grid: return 0
        rows, cols = len(grid), len(grid[0])
        
        dp = [[0]*cols for _ in range(rows)]
        dp[0][0] = grid[0][0]
        
        
        for i in range(1,rows):
            dp[i][0]= dp[i-1][0]+grid[i][0]
            
        for j in range(1, cols):
            dp[0][j] = dp[0][j-1]+grid[0][j]
            
        for i in range(1,rows):
            for j in range(1, cols):
                dp[i][j] = grid[i][j] + min(dp[i-1][j],dp[i][j-1])
        
        return dp[-1][-1]
```

```python
class Solution:
    def minPathSum(self, grid: List[List[int]]) -> int:
        if not grid: return 0
        rows, cols = len(grid), len(grid[0])
        
        for i in range(1,rows):
            grid[i][0] += grid[i-1][0]
            
        for j in range(1, cols):
            grid[0][j] += grid[0][j-1]
            
        for i in range(1,rows):
            for j in range(1, cols):
                grid[i][j] += min(grid[i-1][j],grid[i][j-1])
        
        return grid[-1][-1]
```







### 300 Longest Increasing Subsequence

Given an integer array `nums`, return the length of the longest strictly increasing subsequence.

A **subsequence** is a sequence that can be derived from an array by deleting some or no elements without changing the order of the remaining elements. For example, `[3,6,2,7]` is a subsequence of the array `[0,3,1,6,2,2,7]`.



+ Example:

  + ```
    Input: nums = [10,9,2,5,3,7,101,18]
    Output: 4
    Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.
    ```

  + ```
    Input: nums = [0,1,0,3,2,3]
    Output: 4 (0,1,2,3)
    ```

  + ```
    Input: nums = [7,7,7,7,7,7,7]
    Output: 1
    ```

+ Analysis

  + Recursion: each step is a combination of taking current element and not taking:

    + branch for each taken/ not taken. $O(2^n)$

    ```python
    def lengthofLIS(nums, prevMax, curIdx):
        if curIdx == len(nums): return 0
        taken = 0
        if nums[curIdx]>prevMax:
            taken = 1+lengthofLIS(nums, nums[curIdx],curIdx+1)
        notTaken = lengthofLIS(nums, prev,curIdx+1)
        return max(taken, notTaken)
    ```

  + Recursion with memorization: `memo[prevMax, curIdx]`: $O(n^2)$

    ```python
    memo[i][j]
    def lengthofLIS(nums, prevMax, curIdx):
        if memo[prevMax, curIdx]>-1: return memo[prevMax, curIdx]
        if curIdx == len(nums): return 0
        taken = 0
        if nums[curIdx]>prevMax:
            taken = 1+lengthofLIS(nums, nums[curIdx],curIdx+1)
        notTaken = lengthofLIS(nums, prev,curIdx+1)
        return max(taken, notTaken)
    ```

  + DP: if we know the all max lengths before `i`,  we can try appending `i` to each elements before it and find the max result. $O(n^2)$

    + ```python
      curMax = 0
      for j in range(i):
          if nums[i]>nums[j]:
              curMax = max(curMax, dp[j]+1)
      ```

    + 

  

  |      |      | 10   | 9    | 2    | 5    | 3    | 7    | 101  | 18   |
  | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
  |      |      | 0    | 1    | 2    | 3    | 4    | 5    | 6    | 7    |
  |      |      | 1    |      |      |      |      |      |      |      |
  |      |      | 1    | 1    |      |      |      |      |      |      |
  |      |      | 1    | 1    | 1    |      |      |      |      |      |
  |      |      | 1    | 1    | 1    | 2    |      |      |      |      |
  |      |      | 1    | 1    | 1    | 2    | 2    |      |      |      |
  |      |      | 1    | 1    | 1    | 2    | 2    | 3    |      |      |
  |      |      | 1    | 1    | 1    | 2    | 2    | 3    | 4    |      |
  |      |      | 1    | 1    | 1    | 1    | 2    | 3    | 4    | 4    |

  ```python
  import sys
  
  class Solution:
      def lengthOfLIS(self, nums: List[int]) -> int:
          if not nums: return 0
          
          
          length = len(nums)
          dp = [sys.maxsize]*length
          dp[0] = 1
          res = 1
          for i in range(1,length):
              curMax = 0
              for j in range(i):
                  if nums[i]>nums[j]:
                      curMax = max(dp[j]+1, curMax)
              dp[i] = max(curMax, 1)
              res = max(res, dp[i])
              
          return res
  ```

  

  ```java
  class Solution {
      public int lengthOfLIS(int[] nums) {
          int n = nums.length;
          if(n==0)
              return 0;
          int[] dp = new int[n];
          dp[0] = 1;
          for(int i =1; i<n;i++){
              dp[i] = 1;
              for(int j = 0; j<i;j++){
                  if(nums[j]<nums[i]){
                      dp[i] = Math.max(dp[i], dp[j]+1);
                  }
                  
              }
          }
          return Arrays.stream(dp).max().getAsInt();
      }
  }
  ```

  

  + DP: binary search (patient sort algorithm). With patient sort, the number of piles equal to the longest increasing sequence, because:
  + each pile is a decreasing sequence
    + patient sort always append to smaller pile first
    + $O(n log n)$

  ```python
  last_digit = []
  for num in nums:
      insert_pos = bisect.bisect_left(last_digits,num)
      if insert_pos>=len(last_digits):
          last_digits.append(num)
      else:
          last_digits[insert_pos] = num
  return len(last_digits)
  ```

  https://leetcode.com/problems/longest-increasing-subsequence/discuss/1000066/Python-O(nlogn)-with-patience-sort-explanation-faster-than-92.4

+ Tips:

  + Useful for recursion:

  ```python
  len(array, curIdx) = 1+len(array, curIdx+1)
  ```

  

### 516. Longest Palindromic Subsequence

Medium

Given a string s, find the longest palindromic subsequence's length in s. You may assume that the maximum length of s is 1000.

**Example 1:**
Input:

```
"bbbbab"
```

Output:

```
4
```

One possible longest palindromic subsequence is "bbbb".

 

**Example 2:**
Input:

```
"cbbd"
```

Output:

```
2
```

One possible longest palindromic subsequence is "bb".

+ for subsequence

+ ```python
  class Solution:
      def longestPalindromeSubseq(self, s: str) -> int:
          if not s: return 0
          n = len(s)
          dp = [[0]*n for _ in range(n)]
          for k in range(1,n+1):
              for i in range(0, n-k+1):
                  j = i + k - 1
                  if i == j:
                      dp[i][j] = 1
                  elif s[i] == s[j]: 
                      dp[i][j] = dp[i+1][j-1] + 2
                  else:
                      dp[i][j] = max(dp[i+1][j], dp[i][j-1])
          
          return dp[0][n-1]
  
  class Solution:
      def longestPalindromeSubseq(self, s: str) -> int:
          if not s: return 0
          n = len(s)
          dp = [0]*n
          prev = prev2 = dp
          for k in range(1,n+1):
              prev, prev2 = dp.copy(), prev.copy()
              for i in range(0, n-k+1):
                  j = i + k - 1
                  if i == j:
                      dp[i] = 1
                  elif s[i] == s[j]: 
                      dp[i] = prev2[i+1] + 2 #prev2: k-2
                  else:
                      dp[i] = max(prev[i+1], prev[i]) # prev: k-1
          
          return dp[0]
  ```

+ ```java
  class Solution {
      public int longestPalindromeSubseq(String s) {
          if (s.length() == 0)
              return 0;
          int n = s.length();
          int[][] dp = new int[n][n];
          for (int k  = 1; k<=n; k++){
              for (int i = 0; i <= n-k; i++){
                  int j = i + k - 1;
                  if (i == j){
                      dp[i][j] = 1;
                  }else if (s.charAt(i) == s.charAt(j)){
                      dp[i][j] = dp[i+1][j-1] + 2;
                  }else{
                      dp[i][j] = Math.max(dp[i+1][j], dp[i][j-1]);
                  }
              }
          }
          return dp[0][n-1];
          
      }
  }
  ```

+ 

`dp[start][end]`

```python
    if not s: return 0
    n = len(s)

    dp = [[0]*n for _ in range(n)]
    for i in range(n):
        dp[i][i]=1

    for i in range(n-1, -1, -1):
        for j in range(i+1,n):
            if s[i]==s[j]:
                dp[i][j]= dp[i+1][j-1]+2
            else:
                dp[i][j] = max(dp[i+1][j], dp[i][j-1])

    return dp[0][-1]
```



### 746. Min Cost Climbing Stairs

Easy

On a staircase, the `i`-th step has some non-negative cost `cost[i]` assigned (0 indexed).

Once you pay the cost, you can either climb one or two steps. You need to find minimum cost to reach the top of the floor, and you can either start from the step with index 0, or the step with index 1.

**Example 1:**

```
Input: cost = [10, 15, 20]
Output: 15
Explanation: Cheapest is start on cost[1], pay that cost and go to the top.
```



**Example 2:**

```
Input: cost = [1, 100, 1, 1, 1, 100, 1, 1, 100, 1]
Output: 6
Explanation: Cheapest is start on cost[0], and only step on 1s, skipping cost[3].
```



**Note:**

1. `cost` will have a length in the range `[2, 1000]`.
2. Every `cost[i]` will be an integer in the range `[0, 999]`.

|      | 1    | 100  | 1    | 1    | 1    | 100  | 1    | 1    | 100  | 1    |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| 0    | 1    |      |      |      |      |      |      |      |      |      |
| 0    | 1    | 100  |      |      |      |      |      |      |      |      |
| 0    | 1    | 100  | 2    |      |      |      |      |      |      |      |
| 0    | 1    | 100  | 2    | 3    |      |      |      |      |      |      |
| 0    | 1    | 100  | 2    | 3    | 3    | 100  | 4    | 5    | 100  | 6    |
|      |      |      |      |      |      |      |      |      |      |      |
|      |      |      |      |      |      |      |      |      |      |      |





### 256. Paint House

Medium

There is a row of *n* houses, where each house can be painted one of three colors: red, blue, or green. The cost of painting each house with a certain color is different. You have to paint all the houses such that no two adjacent houses have the same color.

The cost of painting each house with a certain color is represented by a `*n* x *3*` cost matrix. For example, `costs[0][0]` is the cost of painting house 0 with the color red; `costs[1][2]` is the cost of painting house 1 with color green, and so on... Find the minimum cost to paint all houses.

 

**Example 1:**

```
Input: costs = [[17,2,17],[16,16,5],[14,3,19]]
Output: 10
Explanation: Paint house 0 into blue, paint house 1 into green, paint house 2 into blue.
Minimum cost: 2 + 5 + 3 = 10.
```

**Example 2:**

```
Input: costs = []
Output: 0
```

**Example 3:**

```
Input: costs = [[7,6,2]]
Output: 2
```

 

```python
    if not costs: return 0
    n = len(costs)
    dp = [[0,0,0] for _ in range(n)]
    dp[0] = costs[0]

    for i in range(1, n):
        dp[i][0] = costs[i][0]+min(dp[i-1][1],dp[i-1][2])
        dp[i][1] = costs[i][1]+min(dp[i-1][0],dp[i-1][2])
        dp[i][2] = costs[i][2]+min(dp[i-1][1],dp[i-1][0])

    return min(dp[-1][0], dp[-1][1],dp[-1][2])
```



### 87. Scramble String

Hard

We can scramble a string s to get a string t using the following algorithm:

1. If the length of the string is 1, stop.
2. If the length of the string is > 1, do the following:
   - Split the string into two non-empty substrings at a random index, i.e., if the string is `s`, divide it to `x` and `y` where `s = x + y`.
   - **Randomly** decide to swap the two substrings or to keep them in the same order. i.e., after this step, `s` may become `s = x + y` or `s = y + x`.
   - Apply step 1 recursively on each of the two substrings `x` and `y`.

Given two strings `s1` and `s2` of **the same length**, return `true` if `s2` is a scrambled string of `s1`, otherwise, return `false`.

 

**Example 1:**

```
Input: s1 = "great", s2 = "rgeat"
Output: true
Explanation: One possible scenario applied on s1 is:
"great" --> "gr/eat" // divide at random index.
"gr/eat" --> "gr/eat" // random decision is not to swap the two substrings and keep them in order.
"gr/eat" --> "g/r / e/at" // apply the same algorithm recursively on both substrings. divide at ranom index each of them.
"g/r / e/at" --> "r/g / e/at" // random decision was to swap the first substring and to keep the second substring in the same order.
"r/g / e/at" --> "r/g / e/ a/t" // again apply the algorithm recursively, divide "at" to "a/t".
"r/g / e/ a/t" --> "r/g / e/ a/t" // random decision is to keep both substrings in the same order.
The algorithm stops now and the result string is "rgeat" which is s2.
As there is one possible scenario that led s1 to be scrambled to s2, we return true.
```

**Example 2:**

```
Input: s1 = "abcde", s2 = "caebd"
Output: false
```

**Example 3:**

```
Input: s1 = "a", s2 = "a"
Output: true
```

 ![image-20210127092748116](/home/arkyyang/files/notes/notes/attachments/image-20210127092748116.png)

```python
class Solution:
    def isScramble(self, s1: str, s2: str) -> bool:
        if len(s1)!=len(s2): return False
        if s1==s2: return True
        
        n = len(s1)
        
        dp = [[[False]*(n+1) for _ in range(n)] for _ in range(n)]
        for i in range(n):
            for j in range(n):
                dp[i][j][1] = (s1[i]==s2[j])
        
        for length in range(2,n+1):
            for i in range(n):
                for j in range(n):
                    for k in range(1, length):
                        if i+k>=n or j+k>=n or j+length-k>=n: continue
                        dp[i][j][length] = dp[i][j][length] or (dp[i][j][k] and dp[i+k][j+k][length-k]) or (dp[i][j+length-k][k] and dp[i+k][j][length-k])
        
        return dp[0][0][n]
```



### 5. Longest Palindromic Substring

Medium



Given a string `s`, return *the longest palindromic substring* in `s`.

 

**Example 1:**

```
Input: s = "babad"
Output: "bab"
Note: "aba" is also a valid answer.
```

**Example 2:**

```
Input: s = "cbbd"
Output: "bb"
```

**Example 3:**

```
Input: s = "a"
Output: "a"
```

**Example 4:**

```
Input: s = "ac"
Output: "a"
```

 

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        if not s: return ''
        n = len(s)
        maxStr = s[0]
        for i in range(n):
            exp = 1
            while i-exp>=0 and i+exp<n and s[i-exp] == s[i+exp]:
                exp += 1
            if 1+2*(exp-1)>len(maxStr):
                maxStr = s[i-exp+1:i+exp]
            exp = 0
            while i-exp>=0 and i+exp+1<n and s[i-exp] == s[i+exp+1]:
                exp += 1
            if 2*exp > len(maxStr):
                maxStr = s[i-exp+1: i+exp+1]
        
        return maxStr
```



### 42. Trapping Rain Water

Hard

9674149Add to ListShare

Given `n` non-negative integers representing an elevation map where the width of each bar is `1`, compute how much water it can trap after raining.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2018/10/22/rainwatertrap.png)

```
Input: height = [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
Explanation: The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.
```

**Example 2:**

```
Input: height = [4,2,0,3,2,5]
Output: 9
```



```python
class Solution:
    def trap(self, height: List[int]) -> int:
        if not height: return 0
        
        n = len(height)
        
        leftMax = [0]*n
        
        curMax = 0
        for i in range(n):
            curMax = max(curMax, height[i])
            leftMax[i] = curMax
            
        rightMax = 0
        res = 0
        for i in range(n-1,-1,-1):
            rightMax = max(rightMax, height[i])
            res += min(leftMax[i], rightMax) - height[i]
        
        return res
            
```



### 1335. Minimum Difficulty of a Job Schedule

Hard

You want to schedule a list of jobs in `d` days. Jobs are dependent (i.e To work on the `i-th` job, you have to finish all the jobs `j` where `0 <= j < i`).

You have to finish **at least** one task every day. The difficulty of a job schedule is the sum of difficulties of each day of the `d` days. The difficulty of a day is the maximum difficulty of a job done in that day.

Given an array of integers `jobDifficulty` and an integer `d`. The difficulty of the `i-th` job is `jobDifficulty[i]`.

Return *the minimum difficulty* of a job schedule. If you cannot find a schedule for the jobs return **-1**.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/01/16/untitled.png)

```
Input: jobDifficulty = [6,5,4,3,2,1], d = 2
Output: 7
Explanation: First day you can finish the first 5 jobs, total difficulty = 6.
Second day you can finish the last job, total difficulty = 1.
The difficulty of the schedule = 6 + 1 = 7 
```

**Example 2:**

```
Input: jobDifficulty = [9,9,9], d = 4
Output: -1
Explanation: If you finish a job per day you will still have a free day. you cannot find a schedule for the given jobs.
```

**Example 3:**

```
Input: jobDifficulty = [1,1,1], d = 3
Output: 3
Explanation: The schedule is one job per day. total difficulty will be 3.
```

**Example 4:**

```
Input: jobDifficulty = [7,1,7,1,7,1], d = 3
Output: 15
```

**Example 5:**

```
Input: jobDifficulty = [11,111,22,222,33,333,44,444], d = 6
Output: 843
```





```python
dp[day][j] = min(dp[day - 1][i -1] + max(difficulty[i:j])) for i in 0...j


```

```python
    def minDifficulty(self, jobDifficulty: List[int], d: int) -> int:
        n = len(jobDifficulty)
        if n < d:
            return -1
        
        dp = [[0] + [float('inf')] * n for _ in range(d + 1)]
        
        for i in range(1, d + 1):
            for j in range(i, n + 1):
                current = 0
                for k in range(j, i - 1, -1):
                    current = max(current, jobDifficulty[k - 1])
                    dp[i][j] = min(dp[i][j], current + dp[i - 1][k - 1])
        return dp[-1][-1]
```





### 221. Maximal Square

Medium

Given an `m x n` binary `matrix` filled with `0`'s and `1`'s, *find the largest square containing only* `1`'s *and return its area*.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/11/26/max1grid.jpg)

```
Input: matrix = [["1","0","1","0","0"],["1","0","1","1","1"],["1","1","1","1","1"],["1","0","0","1","0"]]
Output: 4
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/11/26/max2grid.jpg)

```
Input: matrix = [["0","1"],["1","0"]]
Output: 1
```

**Example 3:**

```
Input: matrix = [["0"]]
Output: 0
```

 

```python
class Solution:
    def maximalSquare(self, matrix: List[List[str]]) -> int:
        if not matrix: return 0
        maxSqure = 0
        n,m = len(matrix), len(matrix[0])
                  
        curMax = 0
        
        for i in range(0,n):
            if matrix[i][0]=='1':
                curMax = 1
        for j in range(0,m):
            if matrix[0][j]=='1':
                curMax = 1
                
        for i in range(1,n):
            for j in range(1,m):
                if matrix[i][j]=='1':
                    matrix[i][j] = min(int(matrix[i][j-1]), int(matrix[i-1][j-1]), int(matrix[i-1][j]))+1
                    curMax = max(curMax, matrix[i][j])
        

        return curMax*curMax
                    
```



### 312. Burst Balloons

Hard



You are given `n` balloons, indexed from `0` to `n - 1`. Each balloon is painted with a number on it represented by an array `nums`. You are asked to burst all the balloons.

If you burst the `ith` balloon, you will get `nums[i - 1] * nums[i] * nums[i + 1]` coins. If `i - 1` or `i + 1` goes out of bounds of the array, then treat it as if there is a balloon with a `1` painted on it.

Return *the maximum coins you can collect by bursting the balloons wisely*.

 

**Example 1:**

```
Input: nums = [3,1,5,8]
Output: 167
Explanation:
nums = [3,1,5,8] --> [3,5,8] --> [3,8] --> [8] --> []
coins =  3*1*5    +   3*5*8   +  1*3*8  + 1*8*1 = 167
```

**Example 2:**

```
Input: nums = [1,5]
Output: 10
```

 

**Constraints:**

- `n == nums.length`
- `1 <= n <= 500`
- `0 <= nums[i] <= 100`



+ l, r, k (length to pick)

  + ex: 3158-> 
    + `3*1, 158`
    + `3*1*5, 358`

  `dp[l][r]=max(s[i-1]*s[i]*s[i+1]+dp[l][i-1]+dp[i+1][r] for i in len(s)) `



```python
class Solution:
    def maxCoins(self, nums: List[int]) -> int:
        nums = [1]+ nums + [1]
        n = len(nums)
        dp = [[0]*n for _ in range(n)]
        
        for left in range(n-2,-1,-1):
            for right in range(left+2, n):
                dp[left][right] = max(nums[left]*nums[i]*nums[right] + dp[left][i]+dp[i][right] for i in range(left+1, right))
        
        return dp[0][n-1]
```



### 647. Palindromic Substrings

Medium

Given a string, your task is to count how many palindromic substrings in this string.

The substrings with different start indexes or end indexes are counted as different substrings even they consist of same characters.

**Example 1:**

```
Input: "abc"
Output: 3
Explanation: Three palindromic strings: "a", "b", "c".
```

 

**Example 2:**

```
Input: "aaa"
Output: 6
Explanation: Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa".
```

 

+ From each center i, expand 
  + each time expand: +1
  + odd and even case

```python
class Solution:
    def countSubstrings(self, s: str) -> int:
        if not s: return 0
        n = len(s)
        res = 0
        for i in range(n):
            l=r=i
            while l>=0 and r<n and s[l]==s[r]:
                res += 1
                l -= 1
                r += 1
            l = i
            r = i+1
            while l>=0 and r<n and s[l]==s[r]:
                res += 1
                l -= 1
                r += 1
        return res
```



### 10. Regular Expression Matching

Hard

Given an input string (`s`) and a pattern (`p`), implement regular expression matching with support for `'.'` and `'*'` where:

- `'.'` Matches any single character.
- `'*'` Matches zero or more of the preceding element.

The matching should cover the **entire** input string (not partial).

 

**Example 1:**

```
Input: s = "aa", p = "a"
Output: false
Explanation: "a" does not match the entire string "aa".
```

**Example 2:**

```
Input: s = "aa", p = "a*"
Output: true
Explanation: '*' means zero or more of the preceding element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".
```

**Example 3:**

```
Input: s = "ab", p = ".*"
Output: true
Explanation: ".*" means "zero or more (*) of any character (.)".
```

**Example 4:**

```
Input: s = "aab", p = "c*a*b"
Output: true
Explanation: c can be repeated 0 times, a can be repeated 1 time. Therefore, it matches "aab".
```

**Example 5:**

```
Input: s = "mississippi", p = "mis*is*p*."
Output: false
```

 

+ Append $ to both expression to signify the end of the word, Split regular expression by `*`
+ start from back of s, match char group by group, `i` is the right end of each group, j'th group, length of split is m
+ `j>=m-2: match directly`
+ `j<m-2: match j or j+1`







### 1235. Maximum Profit in Job Scheduling

Hard

We have `n` jobs, where every job is scheduled to be done from `startTime[i]` to `endTime[i]`, obtaining a profit of `profit[i]`.

You're given the `startTime` , `endTime` and `profit` arrays, you need to output the maximum profit you can take such that there are no 2 jobs in the subset with overlapping time range.

If you choose a job that ends at time `X` you will be able to start another job that starts at time `X`.

 

**Example 1:**

**![img](https://assets.leetcode.com/uploads/2019/10/10/sample1_1584.png)**

```
Input: startTime = [1,2,3,3], endTime = [3,4,5,6], profit = [50,10,40,70]
Output: 120
Explanation: The subset chosen is the first and fourth job. 
Time range [1-3]+[3-6] , we get profit of 120 = 50 + 70.
```

**Example 2:**

**![img](https://assets.leetcode.com/uploads/2019/10/10/sample22_1584.png)**

```
Input: startTime = [1,2,3,4,6], endTime = [3,5,10,6,9], profit = [20,20,100,70,60]
Output: 150
Explanation: The subset chosen is the first, fourth and fifth job. 
Profit obtained 150 = 20 + 70 + 60.
```

**Example 3:**

**![img](https://assets.leetcode.com/uploads/2019/10/10/sample3_1584.png)**

```
Input: startTime = [1,1,1], endTime = [2,3,4], profit = [5,6,4]
Output: 6
```



```python
        jobs = sorted(zip(startTime, endTime, profit), key= lambda v: v[1])
        
        dp = [[0,0]]
        
        def find(time):
            i = bisect_left(dp, [time+1]) -1
            return dp[i][1]
        
        for s,e,p in jobs:
            dp.append([e, max(find(e), find(s)+p)])
            
        return dp[-1][1]
```



### LC 123.Best Time to Buy and Sell Stock III

Say you have an array for which the *i*th element is the price of a given stock on day *i*.

Design an algorithm to find the maximum profit. You may complete at most *two* transactions.

**Note:** You may not engage in multiple transactions at the same time (i.e., you must sell the stock before you buy again).

 

**Example 1:**

```
Input: prices = [3,3,5,0,0,3,1,4]
Output: 6
Explanation: Buy on day 4 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.
Then buy on day 7 (price = 1) and sell on day 8 (price = 4), profit = 4-1 = 3.
```

**Example 2:**

```
Input: prices = [1,2,3,4,5]
Output: 4
Explanation: Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
Note that you cannot buy on day 1, buy on day 2 and sell them later, as you are engaging multiple transactions at the same time. You must sell before buying again.
```

**Example 3:**

```
Input: prices = [7,6,4,3,1]
Output: 0
Explanation: In this case, no transaction is done, i.e. max profit = 0.
```

**Example 4:**

```
Input: prices = [1]
Output: 0
```

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        if not prices: return 0
        n = len(prices)
        dp = [[0]*4 for _ in range(n)]
        dp[0][0] = dp[0][1]= dp[0][2] = dp[0][3] = -prices[0]
        
        for i in range(1,n):
            dp[i][0] = max(dp[i-1][0], -prices[i]) #have1
            dp[i][1] = max(dp[i-1][1], dp[i-1][0]+prices[i]) # no1
            dp[i][2] = max(dp[i-1][2], dp[i-1][1]-prices[i]) #have2
            dp[i][3] = max(dp[i-1][3], dp[i-1][2]+prices[i]) #no 2         
        return max(max(dp[-1]),0)
```

```java
class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length;
        if (n == 0)
            return 0;
        int[][] dp = new int[n][4];
        dp[0][0] = -prices[0];
        dp[0][1] = -prices[0];
        dp[0][2] = -prices[0];
        dp[0][3] = -prices[0];
        
        for (int i = 1; i<n; i++){
            dp[i][0] = Math.max(dp[i-1][0], -prices[i]);
            dp[i][1] = Math.max(dp[i-1][1], dp[i-1][0]+prices[i]); 
            dp[i][2] = Math.max(dp[i-1][2], dp[i-1][1]-prices[i]); 
            dp[i][3] = Math.max(dp[i-1][3], dp[i-1][2]+prices[i]); 
        }
        return Math.max(Math.max(dp[n-1][1], dp[n-1][3]),0);
        
    }
}
```



### 376. Wiggle Subsequence

Medium

A sequence of numbers is called a **wiggle sequence** if the differences between successive numbers strictly alternate between positive and negative. The first difference (if one exists) may be either positive or negative. A sequence with fewer than two elements is trivially a wiggle sequence.

For example, `[1,7,4,9,2,5]` is a wiggle sequence because the differences `(6,-3,5,-7,3)` are alternately positive and negative. In contrast, `[1,4,7,2,5]` and `[1,7,4,5,5]` are not wiggle sequences, the first because its first two differences are positive and the second because its last difference is zero.

Given a sequence of integers, return the length of the longest subsequence that is a wiggle sequence. A subsequence is obtained by deleting some number of elements (eventually, also zero) from the original sequence, leaving the remaining elements in their original order.

**Example 1:**

```
Input: [1,7,4,9,2,5]
Output: 6
Explanation: The entire sequence is a wiggle sequence.
```

**Example 2:**

```
Input: [1,17,5,10,13,15,10,5,16,8]
Output: 7
Explanation: There are several subsequences that achieve this length. One is [1,17,10,13,10,16,8].
```

**Example 3:**

```
Input: [1,2,3,4,5,6,7,8,9]
Output: 2
```

```java
class Solution {
    public int wiggleMaxLength(int[] nums) {
        int n = nums.length;
        if (n<=1)
            return n;
        int[] up = new int[n];
        int[] down = new int[n];
        up[0] = 1;
        down[0] = 1;
        for (int i = 1; i<n; i++){
            if(nums[i]<nums[i-1]){
                down[i] = up[i-1] + 1;
            }else{
                down[i] = down[i-1];
            }
            if(nums[i]>nums[i-1]){
                up[i] = down[i-1] + 1;
            }else{
                up[i] = up[i-1];
            }
        }
        return Math.max(down[n-1], up[n-1]);
    }
}
```



### 1289. Minimum Falling Path Sum II

Hard

Given a square grid of integers `arr`, a *falling path with non-zero shifts* is a choice of exactly one element from each row of `arr`, such that no two elements chosen in adjacent rows are in the same column.

Return the minimum sum of a falling path with non-zero shifts.

 

**Example 1:**

```
Input: arr = [[1,2,3],[4,5,6],[7,8,9]]
Output: 13
Explanation: 
The possible falling paths are:
[1,5,9], [1,5,7], [1,6,7], [1,6,8],
[2,4,8], [2,4,9], [2,6,7], [2,6,8],
[3,4,8], [3,4,9], [3,5,7], [3,5,9]
The falling path with the smallest sum is [1,5,7], so the answer is 13.
```

```java
class Solution {
    public int minFallingPathSum(int[][] arr) {
        int n = arr.length;
        if(n==0)
            return 0;
        int m = arr[0].length;
        int[] dp = new int[n];
        for (int i=0; i<m; i++){
            dp[i] = arr[0][i];
        }
        for(int i=1; i<n; i++ ){
            int[] prev = dp.clone();
            for(int j=0; j<m; j++){
                int min = Integer.MAX_VALUE;
                for (int k=0; k<m; k++){
                    if(j!=k){
                         min = Math.min(min, prev[k]+arr[i][j]); 
                    }

                }
                dp[j] = min;
            }
        }
        return Arrays.stream(dp).min().getAsInt();
    }
}
```

+ Can be improved by memorizing max and second max value of each row



### 487. Max Consecutive Ones II

Medium

Given a binary array, find the maximum number of consecutive 1s in this array if you can flip at most one 0.

**Example 1:**

```
Input: [1,0,1,1,0]
Output: 4
Explanation: Flip the first zero will get the the maximum number of consecutive 1s.
    After flipping, the maximum number of consecutive 1s is 4.
```

```java
class Solution {
    public int findMaxConsecutiveOnes(int[] nums) {
        int n = nums.length;
        if (n==0)
            return 0;
        int[] noFlip = new int[n];
        int[] flip = new int[n];
        noFlip[0] = nums[0];
        flip[0] = 1;
        for (int i = 1; i<n; i++){
            if(nums[i]==1){
                noFlip[i] = noFlip[i-1]+1;
                flip[i] = flip[i-1]+1;
            }else{
                noFlip[i] = 0;
                flip[i] = noFlip[i-1]+1;
            }
        }
        return Arrays.stream(flip).max().getAsInt();
    }
}
```



### 1186. Maximum Subarray Sum with One Deletion

Medium

Given an array of integers, return the maximum sum for a **non-empty** subarray (contiguous elements) with at most one element deletion. In other words, you want to choose a subarray and optionally delete one element from it so that there is still at least one element left and the sum of the remaining elements is maximum possible.

Note that the subarray needs to be **non-empty** after deleting one element.

 

**Example 1:**

```
Input: arr = [1,-2,0,3]
Output: 4
Explanation: Because we can choose [1, -2, 0, 3] and drop -2, thus the subarray [1, 0, 3] becomes the maximum value.
```

**Example 2:**

```
Input: arr = [1,-2,-2,3]
Output: 3
Explanation: We just choose [3] and it's the maximum sum.
```

**Example 3:**

```
Input: arr = [-1,-1,-1,-1]
Output: -1
Explanation: The final subarray needs to be non-empty. You can't choose [-1] and delete -1 from it, then get an empty subarray to make the sum equals to 0.
```

```java
class Solution {
    public int maximumSum(int[] arr) {
        int n = arr.length;
        if(n==1)
            return arr[0];
        int[] de = new int[n];
        int[] no = new int[n];
        no[0] = arr[0];
        de[0] = Math.max(0, arr[0]);
        int max = Integer.MIN_VALUE;
        for(int i = 1; i<n; i++){
            de[i] = Math.max(no[i-1], de[i-1]+ arr[i]);
            max = Math.max(max, de[i]);
            no[i] = Math.max(no[i-1]+arr[i], arr[i]);
            max = Math.max(max, no[i]);
        }
        return max;
    }
}
```

### 673 Number-of-Longest-Increasing-Subsequence





```python
class Solution:
    def findNumberOfLIS(self, nums: List[int]) -> int:
        if not nums: return 0
        n = len(nums)
        maxLen = 1
        dp = [1]*n
        count = [1]*n
        for i in range(1,n):
            for j in range(0,i):
                if nums[j]<nums[i]:
                    if dp[i] == dp[j]+1:
                        count[i] += count[j]
                    elif dp[j]+1 > dp[i]:
                        dp[i] = dp[j]+1
                        count[i] = count[j]
        return sum([count[i] for i in range(n) if dp[i]==max(dp)])
```

### 368. Largest Divisible Subset

Medium

Given a set of **distinct** positive integers `nums`, return the largest subset `answer` such that every pair `(answer[i], answer[j])` of elements in this subset satisfies:

- `answer[i] % answer[j] == 0`, or
- `answer[j] % answer[i] == 0`

If there are multiple solutions, return any of them.

 

**Example 1:**

```
Input: nums = [1,2,3]
Output: [1,2]
Explanation: [1,3] is also accepted.
```

**Example 2:**

```
Input: nums = [1,2,4,8]
Output: [1,2,4,8]
```

```python
for i in range(1,n):
    for j in range(0,i):
        if nums[i]%nums[j] == 0:
            dp[i] = max(dp[i], dp[j]+1)
            if dp[i] == dp[j]+1:
                sets[i] = sets[j] + [nums[i]]
    if dp[i]> maxVal:
        maxVal, maxIdx = dp[i], i
return sets[maxIdx]
```

