### 71. Simplify Path

Medium

Given an **absolute path** for a file (Unix-style), simplify it. Or in other words, convert it to the **canonical path**.

In a UNIX-style file system, a period `'.'` refers to the current directory. Furthermore, a double period `'..'` moves the directory up a level.

Note that the returned canonical path must always begin with a slash `'/'`, and there must be only a single slash `'/'` between two directory names. The last directory name (if it exists) **must not** end with a trailing `'/'`. Also, the canonical path must be the **shortest** string representing the absolute path.

 

**Example 1:**

```
Input: path = "/home/"
Output: "/home"
Explanation: Note that there is no trailing slash after the last directory name.
```

**Example 2:**

```
Input: path = "/../"
Output: "/"
Explanation: Going one level up from the root directory is a no-op, as the root level is the highest level you can go.
```

**Example 3:**

```
Input: path = "/home//foo/"
Output: "/home/foo"
Explanation: In the canonical path, multiple consecutive slashes are replaced by a single one.
```

**Example 4:**

```
Input: path = "/a/./b/../../c/"
Output: "/c"
```



+ Use stack. Cases:
  + each step `stack.append(/cur)`
  + `if cur=='/', continue`
  + `if cur=='.'`: ` continue`
  + `if cur=='..'`: `if stack: stack.pop()`
  + if end,  skip last ``'/'``

```python
    if not path: return path
    if path[-1]!='/': path+='/'
    stack = []
    length = len(path)
    cur=''
    for i in range(length):
        if path[i]=='/':
            if cur: 
                if cur=='..': 
                    if stack: 
                        stack.pop()
                elif cur!='.':
                    stack.append(cur)
            cur=''
            continue
        cur+=path[i]
    return '/'+'/'.join(stack)
```

```python
# more concise implementation:
def simplifyPath(self, path):
        z = path.split("/")
        z = filter(lambda x: x != "" and x != ".",z)
        s = list();
        for x in z:
        	if(x != ".."):
        		s.append(x)
        	elif s:
        		s.pop()
        
        result = "/".join(s)
        return "/"+result
```



+ Tips:
  + use `for section in path.split('/')`
  + 

### 150. Evaluate Reverse Polish Notation

Medium

1375504Add to ListShare

Evaluate the value of an arithmetic expression in [Reverse Polish Notation](http://en.wikipedia.org/wiki/Reverse_Polish_notation).

Valid operators are `+`, `-`, `*`, `/`. Each operand may be an integer or another expression.

**Note:**

- Division between two integers should truncate toward zero.
- The given RPN expression is always valid. That means the expression would always evaluate to a result and there won't be any divide by zero operation.

**Example 1:**

```
Input: ["2", "1", "+", "3", "*"]
Output: 9
Explanation: ((2 + 1) * 3) = 9
```

**Example 2:**

```
Input: ["4", "13", "5", "/", "+"]
Output: 6
Explanation: (4 + (13 / 5)) = 6
```

**Example 3:**

```
Input: ["10", "6", "9", "3", "+", "-11", "*", "/", "*", "17", "+", "5", "+"]
Output: 22
Explanation: 
  ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
= ((10 * (6 / (12 * -11))) + 17) + 5
= ((10 * (6 / -132)) + 17) + 5
= ((10 * 0) + 17) + 5
= (0 + 17) + 5
= 17 + 5
= 22
```

+ Use a stack. Whenever see operators, pop two values from the stack and operate

```python
import math
class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        def isOp(s):
            return s in ['+','-','*','/']
        def evaluate(n1,n2,op):
            if op=='/':
                res = eval(n1+op+n2)
                if res>0:
                    return math.floor(res)
                else:
                    return math.ceil(res)
            return eval(n1+op+n2)
        
        stack = []
        for i in range(len(tokens)):
            cur = tokens[i]
            if isOp(cur):
                n2 = stack.pop(-1)
                n1 = stack.pop(-1)
                stack.append(str(evaluate(n1,n2,cur)))
            else:
                stack.append(cur)
        return stack[-1]
```

```python
# combining operator identification with operation with dictionary+lambda is fast
def evalRPN(self, tokens: List[str]) -> int:
        
    operations = {
        "+": lambda a, b: a + b,
        "-": lambda a, b: a - b,
        "/": lambda a, b: int(a / b),
        "*": lambda a, b: a * b
    }
    
    stack = []
    for token in tokens:
        if token in operations:
            number_2 = stack.pop()
            number_1 = stack.pop()
            operation = operations[token]
            stack.append(operation(number_1, number_2))
        else:
            stack.append(int(token))
    return stack.pop()
```



### 42. Trapping Rain Water

Hard

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

+ Solution:
  + Brute force:
    + At a given index, the max height of the water is `min(leftmax(i), rightmax(i))`, the max height of bar to its left and right
  + DP:
    + When we iterate to find the max, we can store values on the go. then `leftmax(i) = max(leftmax(i-1), height[i])`. Base case `leftmax(0)=0`
  + Stack:

![image-20210117201142907](/home/arkyyang/files/notes/notes/attachments/image-20210117201142907.png)

![image-20210117201158217](/home/arkyyang/files/notes/notes/attachments/image-20210117201158217.png)



### 1249. Minimum Remove to Make Valid Parentheses

Medium

Given a string s of `'('` , `')'` and lowercase English characters. 

Your task is to remove the minimum number of parentheses ( `'('` or `')'`, in any positions ) so that the resulting *parentheses string* is valid and return **any** valid string.

Formally, a *parentheses string* is valid if and only if:

- It is the empty string, contains only lowercase characters, or
- It can be written as `AB` (`A` concatenated with `B`), where `A` and `B` are valid strings, or
- It can be written as `(A)`, where `A` is a valid string.

 

**Example 1:**

```
Input: s = "lee(t(c)o)de)"
Output: "lee(t(c)o)de"
Explanation: "lee(t(co)de)" , "lee(t(c)ode)" would also be accepted.
```

**Example 2:**

```
Input: s = "a)b(c)d"
Output: "ab(c)d"
```

**Example 3:**

```
Input: s = "))(("
Output: ""
Explanation: An empty string is also valid.
```

**Example 4:**

```
Input: s = "(a(b(c)d)"
Output: "a(b(c)d)"
```



+ Go over the string, find total number of right ')' totRight
+ Count left and right. Cases
  + cur=')':
    + if right>=left: totRight--, pass
    + else +=
  + cur='(':
    + if left>=totRight: pass
    + else+=

```python
class Solution:
    def minRemoveToMakeValid(self, s: str) -> str:
        res=''
        totRight=0
        for c in s:
            if c==')': totRight+=1
        
        left=right=0
        for c in s:
            if c==')':
                if right>=left:
                    totRight-=1
                    continue
                else:
                    right+=1
            elif c=='(':
                if left>=totRight:
                    continue
                else: left+=1
            res+=c
        return res
```

### 394. Decode String

Medium

Given an encoded string, return its decoded string.

The encoding rule is: `k[encoded_string]`, where the `encoded_string` inside the square brackets is being repeated exactly `k` times. Note that `k` is guaranteed to be a positive integer.

You may assume that the input string is always valid; No extra white spaces, square brackets are well-formed, etc.

Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers, `k`. For example, there won't be input like `3a` or `2[4]`.

 

**Example 1:**

```
Input: s = "3[a]2[bc]"
Output: "aaabcbc"
```

**Example 2:**

```
Input: s = "3[a2[c]]"
Output: "accaccacc"
```

**Example 3:**

```
Input: s = "2[abc]3[cd]ef"
Output: "abcabccdcdcdef"
```

**Example 4:**

```
Input: s = "abc3[cd]xyz"
Output: "abccdcdcdxyz"
```

```python
class Solution:
    def decodeString(self, s: str) -> str:
        res=""
        stack=[[1,""]]
        count = 0
        for c in s:
            if c.isnumeric():
                count = count*10+int(c)
            elif c=='[':
                stack.append([count,""])
                count=0
            elif c=="]":
                curCount, curStr = stack.pop()
                stack[-1][-1]+=curCount*curStr
            else:
                stack[-1][-1]+=c
        return stack[-1][-1]
```

### 224. Basic Calculator

Hard

Given a string `s` representing an expression, implement a basic calculator to evaluate it.

 

**Example 1:**

```
Input: s = "1 + 1"
Output: 2
```

**Example 2:**

```
Input: s = " 2-1 + 2 "
Output: 3
```

**Example 3:**

```
Input: s = "(1+(4+5+2)-3)+(6+8)"
Output: 23
```

+ No multiplication or division. Then parenthesis gives priority

```python
class Solution:
    def calculate(self, s: str) -> int:
        stack=[""]
        for c in s:
            if c=='(':
                stack.append("")
            elif c==")":
                last = stack.pop()
                stack[-1]+=str(eval(last))
            else:
                stack[-1]+=c
        return eval(stack[-1])
```

+ Speed up by evaluating on the go (2*speed)

```python
class Solution:
    def calculate(self, s: str) -> int:

        stack = []
        operand = 0
        res = 0 
        sign = 1 

        for ch in s:
            if ch.isdigit():
                operand = (operand * 10) + int(ch)

            elif ch == '+':
                res += sign * operand
                sign = 1
                operand = 0

            elif ch == '-':
                res += sign * operand
                sign = -1
                operand = 0

            elif ch == '(':
                stack.append(res)
                stack.append(sign)
                sign = 1
                res = 0

            elif ch == ')':
                res += sign * operand
                res *= stack.pop()
                res += stack.pop()
                operand = 0

        return res + sign * operand
        
        
```

### 85. Maximal Rectangle

Hard

Given a `rows x cols` binary `matrix` filled with `0`'s and `1`'s, find the largest rectangle containing only `1`'s and return *its area*.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/09/14/maximal.jpg)

```
Input: matrix = [["1","0","1","0","0"],["1","0","1","1","1"],["1","1","1","1","1"],["1","0","0","1","0"]]
Output: 6
Explanation: The maximal rectangle is shown in the above picture.
```

**Example 2:**

```
Input: matrix = []
Output: 0
```

**Example 3:**

```
Input: matrix = [["0"]]
Output: 0
```

**Example 4:**

```
Input: matrix = [["1"]]
Output: 1
```

**Example 5:**

```
Input: matrix = [["0","0"]]
Output: 0
```

+ DP store: max rectangle with right lower corner at current loc
+ DP relation:
  + if  `matrix[i-1][j-1]!=0`:
    + `dp[i][j]=dp[i-1][j]+dp[i][j-1]=dp[i-1][j-1]`
  + `if matrix[i-1][j-1]==0`
    + `dp[i][j]=max(dp[i-1][j],dp[i][j-1])+1`
  + base case:
    + `if matrix[i][j]==0: dp[i][j]==0`
    + `if not legal: 0`
      + (or do padding, but may be slow)
      + go over first row and first column separately: fastest way
    + keep record of max `dp[i][j]`, return that

+ Above approach is too complicated, missing some cases. 
+ Find max rectangle at each point instead 

![image-20210117201354926](/home/arkyyang/files/notes/notes/attachments/image-20210117201354926.png)







### Two Sum (hashMap)

![image-20210116192831320](/home/arkyyang/files/notes/notes/attachments/image-20210116192831320.png)



### 290 Word Pattern

Easy

Given a `pattern` and a string `s`, find if `s` follows the same pattern.

Here **follow** means a full match, such that there is a bijection between a letter in `pattern` and a **non-empty** word in `s`.

 

**Example 1:**

```
Input: pattern = "abba", s = "dog cat cat dog"
Output: true
```

**Example 2:**

```
Input: pattern = "abba", s = "dog cat cat fish"
Output: false
```

**Example 3:**

```
Input: pattern = "aaaa", s = "dog cat cat dog"
Output: false
```

**Example 4:**

```
Input: pattern = "abba", s = "dog dog dog dog"
Output: false
```

![image-20210116192950628](/home/arkyyang/files/notes/notes/attachments/image-20210116192950628.png)



### Group Anagrams

![image-20210116193013582](/home/arkyyang/files/notes/notes/attachments/image-20210116193013582.png)



### 84 largest rectangle in histogram

![image-20210117201232282](/home/arkyyang/files/notes/notes/attachments/image-20210117201232282.png)

![image-20210117201245208](/home/arkyyang/files/notes/notes/attachments/image-20210117201245208.png)

![image-20210117201254796](/home/arkyyang/files/notes/notes/attachments/image-20210117201254796.png)

![image-20210117201305253](/home/arkyyang/files/notes/notes/attachments/image-20210117201305253.png)



### 221 Maximal Square

![image-20210117201322564](/home/arkyyang/files/notes/notes/attachments/image-20210117201322564.png)

![image-20210117201331230](/home/arkyyang/files/notes/notes/attachments/image-20210117201331230.png)





### 503. Next Greater Element II

Medium

Given a circular array (the next element of the last element is the first element of the array), print the Next Greater Number for every element. The Next Greater Number of a number x is the first greater number to its traversing-order next in the array, which means you could search circularly to find its next greater number. If it doesn't exist, output -1 for this number.

**Example 1:**

```
Input: [1,2,1]
Output: [2,-1,2]
Explanation: The first 1's next greater number is 2; The number 2 can't find next greater number; The second 1's next greater number needs to search circularly, which is also 2.
```




​        

```python
class Solution:
    def nextGreaterElements(self, nums: List[int]) -> List[int]:
        if not nums: return []
        nums = nums+nums
        n = len(nums)
        stack=[]
        res = [-1]*n
	for i in range(n):
        if not stack: 
            stack.append([i, nums[i]])
        elif nums[i]<stack[-1][1]:
            stack.append([i,nums[i]])
        else:
            while stack and nums[i]>stack[-1][1]:
                cur = stack.pop()
                res[cur[0]] = nums[i]
            stack.append([i, nums[i]])
    
    return res[:n//2]
```



### Bad hair day

Some of Farmer John's N cows (1 ≤ N ≤ 800) are having a bad hair day! Since each cow is self-conscious about her messy hairstyle, FJ wants to count the number of other cows that can see the top of other cows' heads. Each cow i has a specified height hi (1 ≤ hi ≤ 1,000) and is standing in a line of cows all facing east (to the right in our diagrams). Therefore, cow i can see the tops of the heads of cows in front of her (namely cows i+1, i+2, and so on), for as long as these cows are strictly shorter than cow i.Let ci denote the number of cows whose hairstyle is visible from cow i; please compute the sum of c1 through cN



+ Find res, where `res[i] = range[i,k] where nums[i]..nums[k] all < nums[i]`

```python
    stack = []
    res = 0
    for cow in cows:
        while stack and cow>=stack[-1]:
            stack.pop()
        res += len(stack)
        stack.append(cow)                
    return res
```



### Print all subarrays with 0 sum

- Difficulty Level : [Medium](https://www.geeksforgeeks.org/medium)
-  Last Updated : 09 May, 2020

Given an array, print all subarrays in the array which has sum 0.

**Examples:**

```
Input:  arr = [6, 3, -1, -3, 4, -2, 2,
             4, 6, -12, -7]
Output:  
Subarray found from Index 2 to 4
Subarray found from Index 2 to 6          
Subarray found from Index 5 to 6
Subarray found from Index 6 to 9
Subarray found from Index 0 to 10
```

```python
from collections import defaultdict
def findSubArrays(nums):
    prefixSum = defaultdict(list)
    prefixSum[0].append(-1)
    curSum = 0
    for i, num in enumerate(nums):
        curSum += num
        prefixSum[curSum].append(i)
    res = []
    for idxs in prefixSum.values():
        while len(idxs)>1:
            r = idxs.pop()
            for l in idxs:
                res.append([l+1, r])
    return sorted(res)
```



### 76. Minimum Window Substring

Hard

Given two strings `s` and `t`, return *the minimum window in `s` which will contain all the characters in `t`*. If there is no such window in `s` that covers all characters in `t`, return *the empty string `""`*.

**Note** that If there is such a window, it is guaranteed that there will always be only one unique minimum window in `s`.

 

**Example 1:**

```
Input: s = "ADOBECODEBANC", t = "ABC"
Output: "BANC"
```

**Example 2:**

```
Input: s = "a", t = "a"
Output: "a"
```

 

**Constraints:**

- `1 <= s.length, t.length <= 105`
- `s` and `t` consist of English letters.



![image-20210207115437151](/home/arkyyang/files/notes/notes/attachments/image-20210207115437151.png)

```python
from collections import defaultdict
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        mapS = defaultdict(int)
        mapT = defaultdict(int)
        for c in t:
            mapT[c]+=1
        
        count = 0
        start = 0
        res = ""
        for i,c in enumerate(s):
            if mapT[c]>0:
                mapS[c]+=1
                if mapT[c]>=mapS[c]:
                    count+=1
            if count == len(t):
                while mapT[s[start]]<mapS[s[start]] or mapT[s[start]]==0:
                    if mapT[s[start]]<mapS[s[start]]:
                        mapS[s[start]] -= 1
                    start += 1
                if not res or i-start+1<len(res):
                    res = s[start:i+1]
        return res
```

