# 8 Recursion and Dynamic Programming

### Approach

+ Bottom-Up: start from simple (base) case, key: build solution of one case based off the previous case
+ Top-down: divide problem for case N into subproblems
+ Half-and-half: divide and merge

Get runtime by drawing out the recursive call tree.

#### Improvements

+ Iterative implementation of recursive algorithms is more space efficient. $O(n) \rightarrow O(1)$

### Dynamic Programming

+ Idea: take recursive algorithm and find the overlapping subproblems (repeated calls), cache the results for future recursive calls

#### Examples

##### Fibonacci Numbers

+ Recursive: $O(2^n)$, space $O(n)$ 
  + space complexity is no bigger than the depth of recursion tree

```java
int fibonacci(int i){
    if (i==0) return 0;
    if (i==1) return 1;
    return fibonacci(i-1) + fibonacci(i-2)
}
```

+ Top-down Dynamic Programming (memorization): $O(n)$, space $O(n)$
  + space: still need to go down the tree at least once with full depth. A bit lager than $n$ because need to save the memo array.

```java
int fibonacci(int n){
    return fibonacci(n, new int[n+1]);
}
int fibonacci(int i, int[] memo){
    if(i==0||i==1) return o;
    if(memo[i]==0){
        memo[i] = fibonacci(i-1,memo)+fibonacci(i-2, memo);
    }
    return memo[i];
}
```

+ Bottom-up Dynamic Programming (iterative memorization): space is still $O(n)$ because need to save the memo array. 

```java
int fibonacci(int n){
    if (n==0||n==1) return n;
    int[] memo = new int[n];
    memo[0] = 0;
    memo[1] = 1;
    for(int i =2; i<n; i++){
        memo[i] = memo[i-1]+memo[i-2];
    }
    return memo[n-1]+memo[n-2];
}
```

+ Bottom-up DP without memo array: space $O(1)$
  + Only i-1 and i-2 th elements are used in each iteration, no need to save the whole array

```java
int fibonacci(int n){
    if(n==0) return 0;
    int a = 0;
    int b = 1;
    for(int i=2; i<n;i++){
        int c = a+b;
        a = b;
        b = c;
    }
    return a+b;
}
```



p142



## Interview Questions

### 8.1 Triple Step

A child is running up a staircase with n steps and can hop either 1 step, 2 steps, or 3
steps at a time. Implement a method to count how many possible ways the child can run up the stairs.

+ Draw Example

  + 4 steps: 
    + 1111,112,121,13
    + 211,22
    + 31

+ Brute Force

  + Recursion: 
    + Base case: 0: 0; 1: 1; 2: 2; 3: 4 (111,21,12,3)
    + Recursion: f(n) = f(n-1) + f(n-2) + f(n-3)

+ Optimize + Conceptual algorithm walk through

  + Memorize: memo[n] = f(n)
  + Only uses previous 3 numbers: use a,b,c to memorize previous 3 steps

+ Implement + Test

  + ```java
        public int answer(int N) {
            if(N==0) return 0;
            if(N==1) return 1;
            if(N==2) return 2;
            if(N==3) return 4;
            int a = 1;
            int b = 2;
            int c = 4;
            for(int i = 4; i<N; i++){
                int cur = a+b+c;
                a = b;
                b = c;
                c = cur;
            }
            return a+b+c;
        }
    ```

+ Tips

  + If only uses a few previous values, only memorize those in single variables instead of an array
  + If result get too big and overflows, can use BitInteger class or something else to resolve this issue.

### 8.2 Robot in a Grid

Imagine a robot sitting on the upper left corner of grid with r rows and c columns.
The robot can only move in two directions, right and down, but certain cells are "off limits" such that the robot cannot step on them. Design an algorithm to find a path for the robot from the top left to the bottom right.

+ Draw Example

  + | Start | $\rightarrow$ | $\rightarrow$ | $\rightarrow$ |      |
    | ----- | ------------- | ------------- | ------------- | ---- |
    |       |               | x             | $\downarrow$  |      |
    | x     | x             |               | $\downarrow$  |      |
    |       |               |               | $\rightarrow$ | End  |

  + only right and down: (r-1, c-1)

+ Brute Force

  + Recursion: start from i,j = 0,0
    + if i,j = 0,0, return true
    + if $M[i][j] = 0$ blocked: return false 
    + f(i,j)  = f(i+1,j) || f(i,j+1)
  + $O(2^{r+c})$

+ Optimize + Conceptual algorithm walk through

  + memorize $F[i][j]$: value returned by each recursive call $O(rc)$
  + Only uses f(i+1) and f(j+1), only memorize values related to i+1's row and j+1's column
  + Each $F[i][j]$ are just true/ false value, can keep a hashset of Point(row, col) for false points, fast lookup and then no need to take up space of whole array. - may not be more space efficient than the last approach.

+ Implement + Test

  + Answer:

    ```java
    ArrayList<Point> getPath(boolean[][] maze){
        if(maze==null || maze.length==0) return null;
        ArrayList<Point> path = new ArrayList<Point>();
        HashSet<Point> failedPoints = new HashSet<Point>();
        if (getPath(maze, maze.length-1, maze[0].length-1, path, failedPoints)){
            return path;
        }
        return null;
    }
    
    boolean getPath(boolean[][] maze, int row, int col, ArrayList<Point> path, HashSet<Point> failedPoints){
        if(col<0|| row<0||!maze[row][col]){
            return false;
        }
        Point p = new Point(row, col);
        if(failedPoints.contains(p)){return false;}
        boolean isAtOrigin = (row==0) && (col==0);
        if(isAtOrigin || getPath(maze, row, col-1, path, failedPoints)||getPath(maze, row-1, col-1, path, failedPoints)){
            path.add(p);
            return true;
        }
        failedPoints.add(p);
        return false;
    }
    ```

+ Tips

  + Use a HashSet to memorize in DP saves space.
  + If need to memorize trajectory, recursive call is easier to implement than iterative calls.

### 8.3 Magic Index

A magic index in an array A[1. .. n-1] is defined to be an index such that A[ i]=i. Given a sorted array of distinct integers, write a method to find a magic index, if one exists, in array A.
FOLLOW UP
What if the values are not distinct?

+ Draw Example

  + $[-5,-3,0,1,4,5,6]$: 4

+ Brute Force

  + Iterate through array, look for element that matches it

+ Optimize + Conceptual algorithm walk through

  + Binary search, if middle element < index, then magic index must be on right, else on left, if = then is that.
  + Use iteration rather than recursion to save space

+ Implement + Test

  + Distinct Values:

    ```java
        public int answer(int[] x) {
            int start = 0;
            int end = x.length-1;
            while(start<=end){
                System.out.println(start);
                System.out.println(end);
                int mid = (start+end)/2;
                System.out.println(mid);
                if(x[mid]==mid) return mid;
                if(x[mid]>mid){
                    end = mid-1;
                }else{
                    start = mid+1;
                }
            }
            return -1;
        }
    ```

    

+ Draw Example
  + $[-5,4,4,4,4,4,6]$: 4
  + We cannot conclude if middle is larger than index
+ Brute Force: iterate $O(n)$
+ Optimize + Conceptual algorithm walk through: 
  
  + recurse on both left and right, faster than iteration but still $O(n)$
+ Tips
  
  + To use iteration for divide and conquer, use index start/end/mid to shrink the current region 

### 8.4 Power Set

Write a method to return all subsets of a set.

+ Draw Example

  + (a,b,c): (),(a),(b),(c), (a,b), (a,c),(b,c), (a,b,c)
  + Total $2^n$ sets

+ Brute Force

  + Recursion: P(n) = P(n-1) + {P(n-1) + $a_n$}

+ Optimize + Conceptual algorithm walk through

  + Use set[2^n], convert each index to binary number, the index of binary represents whether an element is in the subset. $O(n2^n)$

+ Implement + Test

  + ```java
    ArrayList<ArrayList<Integer>> geSubsets (ArrayList<Integer> set){
        ArrayList<ArrayList<Integer>> allsubsets = new ArrayList<ArrayList<Integer>>();
        int max = 1 << set.size(); //compute 2^n
        for (int k = 0; k<max; k++){
            ArrayList<Integer> subset = convertIntToSet(k, set);
            allsubsets.add(subset);
        }
        return allsubsets;        
    }
    
    ArrayList<Integer> convertIntToSet(int x, ArrayList<Integer> set){
        ArrayList<Integer> subset = new ArrayList<Integer>();
        int index = 0;
        for(int k = x, k>0; k>>=1){
            if((k&1)==1){
                subset.add(set.get(index));
            }
            index++;
        }
        return subset;
    }
    ```

  + 

+ Tips

  + When encounter $2^n$ problems, use binary number

### 8.5 Recursive Multiply

Write a recursive function to multiply two positive integers without using the * operator (or / operator). You can use addition, subtraction, and bit shifting, but you should minimize the number of those operations.

+ Draw Example

  + 2*3 = 2+2+2 = 3+3
  + 4*8 = 8+8+8+8 = (8+8) + (8+8)

+ Brute Force

+ Optimize + Conceptual algorithm walk through

  + Find max of the 2 numbers, repeat that number (m) the other number(n) of times
  + If n is even, calculate $n/2*m + n/2*m$ , else calculate $m + n/2*m + n/2*m$

+ Implement + Test

  + ```java
        public int multiply(int a, int b) {
            if(a==0||b==0) return 0;
            if(a==1||b==1) return a+b-1;
            int m;
            int n;
            int result = 0;
            if(a>b){
                m = a;
                n = b;
            }else{
                m = b;
                n = a;
            }
            if(n%2==1){
                result +=m;
                n-=1;
            }
            int half = multiply(m,n/2);
            result = result + half+half;
            return result;
        }
    ```

  + 

### 8.6 Towers of Hanoi

In the classic problem of the Towers of Hanoi, you have 3 towers and N disks of
different sizes which can slide onto any tower. The puzzle starts with disks sorted in ascending order of size from top to bottom (Le., each disk sits on top of an even larger one). You have the following constraints:
(1) Only one disk can be moved at a time.
(2) A disk is slid off the top of one tower onto another tower.
(3) A disk cannot be placed on top of a smaller disk.
Write a program to move the disks from the first tower to the last using Stacks

+ Draw Example

  + N = 1: a-> c
  + N = 2: a -> b; a-> c; b->c
  + N = 3: 12-> b; 3->c; 12->c
  + N = 4: 123->b; 4->c; 123->c

+ Implement + Test

  ```java
      void answer(int N) {
          move(N, 1,3,2);
      }
      void move(int n, int start, int target, int buffer){
          if(n<=0) return;
          move(n-1, start, buffer, target);
          moveTop(start, target);
          move(n-1, buffer, target, start);
      }
      void moveTop(int start, int target){
          System.out.println("Moved from "+start+" to "+target);
      }
  ```



### 8.7 Permutations without Dups

Write a method to compute all permutations of a string of unique characters.

+ Draw Example

  + {1,2,3}: 123,132,231,213,312,321

+ Brute Force

  + Start with 1 or 2 or 3, permute the remaining 2 numbers. Recurse with n-1 numbers

+ Implement + Test

  ```java
      ArrayList<String> getPerms(String s)
      {
          int len = s.length();
          ArrayList<String> result = new ArrayList<String>();
          if(len==0) {
              result.add("");
              return result;
          }
          for(int i=0; i<len;i++){
              ArrayList<String> remainders = getPerms(s.substring(0,i) + s.substring(i+1,len));
              for(String remainder: remainders){
                  result.add(s.charAt(i)+remainder);
              }
          }
          return result;
      }
  ```

+ Tips

  + append to ArrayList: `list.add(s)`
  + get i'th char of a string: `s.charAt(i)`
  + when need to expand a list of results by more than one result, create a list of current result and append to that first, then append results from recursive calls to that result





### 8.8 Permutations with Duplicates

Write a method to compute all permutations of a string whose characters are not necessarily unique. The list of permutations should not have duplicates.

+ Draw Example

  + {1,2,2}: 122,212,221

+ Optimize + Conceptual algorithm walk through

  + count each char into a hashtable
  + Only iterate through the keys each recursion
  + Goes until all counts are 0 

+ Implement + Test

  ```java
      HashMap<Character, Integer> buildFreqTable(String s){
          HashMap<Character, Integer> map = new HashMap<Character, Integer>();
          for(char c: s.toCharArray()){
              if(!map.containsKey(c)){
                  map.put(c,0);
              }
              map.put(c,map.get(c)+1);
          }
          return map;
      }
      void printPerms(HashMap<Character, Integer> map, String prefix, int remaining, ArrayList<String> result){
          if(remaining==0){
              result.add(prefix);
              return;
          }
          for(Character c: map.keySet()){
              int count = map.get(c);
              if(count>0){
                  map.put(c, count-1);
                  printPerms(map, prefix+c, remaining-1, result);
                  map.put(c, count);
              }
          }
      }
  ```

  

+ Tips

  + Hashmap: `map.put(key, value)`, `map.keySet()`, `if(map.containsKey(key))`, `value=map.get(key)`
  + recursion call based on iterating over parts of a string:
    `f(prefix, remaining, ArrayList<String> result)`





p371



+ Draw Example
+ Brute Force
+ Optimize + Conceptual algorithm walk through
+ Implement + Test
+ Tips



p354