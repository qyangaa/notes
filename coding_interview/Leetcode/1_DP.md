## 5 Longest Palindromic Substring

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
    
    

