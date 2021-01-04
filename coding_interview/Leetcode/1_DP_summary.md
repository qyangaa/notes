## General Process of Throught

+ Recursion:

  ```python
  for subproblem in direction_of_contraction:
  	f(subproblem)
  ```

+ Recursion with memorization:

  ```python
  memo=[]
  
  for subproblem in direction_of_contraction:
     	if subproblem in memo: return memo[subproblem]
  	f(subproblem)
  ```

  

+ DP: 

  + direction of expansion is the opposite of direction of contraction
  + To go over the subproblems from small to big, follow direction_of_traverse

  ```python
  for problem_size in direction_of_expansion:
      for n in direction_of_traverse:
          storageArray[problem] = f(storageArray[sub_problem])
  ```

+ DP without storage if only need to return extrema:

  ```python
  for n in direction_of_traverse:
      for problem_size in direction_of_expansion:
          expanded_value = expand(sub_problem)
  ```

+ DP without storage if we only use constant steps ahead:

  + (for 1d problem) : store in simple variables

    ```python
    for i in direction_of_expansion:
        cur = f(prev1)+f(prev2)..
        prev1, prev2 = cur, prev1
    ```

  + (for 2d problem): store a 1d array instead of 2d memo

    ```python
    while j < n:
        memocur[j] = memoprev[j] + memocur[j - 1]
        j += 1
    memoprev = memocur
    reset(memocur)
    ```

    

+ Greedy: 

  + If only one good item in inner loop can determine the result
  + Go in the direction inverse to direction of recursion contraction
  + every first good item we encounter in the inverse direction should be the new starting point of the inverse iteration

| Problem                                                      | dir_contraction | dir_expansion       | dir_traverse             |
| ------------------------------------------------------------ | --------------- | ------------------- | ------------------------ |
| [Find palindrome substrings](https://leetcode.com/problems/longest-palindromic-substring/solution/) of (s) | `len(s)--`      | `len(palindrome)++` | `for i in range(len(s))` |
|                                                              |                 |                     |                          |
|                                                              |                 |                     |                          |
|                                                              |                 |                     |                          |
|                                                              |                 |                     |                          |
|                                                              |                 |                     |                          |
|                                                              |                 |                     |                          |
|                                                              |                 |                     |                          |

