## General Process of Throught

+ Recursion:

  ```python
  for subproblem in direction_of_contraction:
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

  

| Problem                                                      |      | direction_of_expansion | direction_of_traverse    |
| ------------------------------------------------------------ | ---- | ---------------------- | ------------------------ |
| [Find palindrome substrings](https://leetcode.com/problems/longest-palindromic-substring/solution/) |      | `len(palindrome)++`    | `for i in range(len(s))` |
|                                                              |      |                        |                          |
|                                                              |      |                        |                          |
|                                                              |      |                        |                          |
|                                                              |      |                        |                          |
|                                                              |      |                        |                          |
|                                                              |      |                        |                          |
|                                                              |      |                        |                          |

