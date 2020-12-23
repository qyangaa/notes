# 10 Sorting and Searching

### Sorting Algorithms

| Algorithm          | Runtime                          | Space       | Approach                                                     | Usage                                |
| ------------------ | -------------------------------- | ----------- | ------------------------------------------------------------ | ------------------------------------ |
| Bubble Sort        | $O(n^2)$                         | $O(1)$      | For each pass, swap pair if out of order                     |                                      |
| Selection Sort     | $O(n^2)$                         | $O(1)$      | For each pass, find smallest element and move to front       |                                      |
| Insertion Sort     | $O(n^2)$                         | $O(1)$      | Each pass, put one element from unsorted part (right) to correct place in sorted part (left) | Good for small set, online           |
| Merge Sort         | $O(n\log n)$                     | Depends     | Merge two sorted sub-arrays recursively                      |                                      |
| Quick Sort         | $O(n\log n)$ avg. $O(n^2)$ worst | $O(log(n))$ | Pick random pivot, put all numbers less than pivot on the left. Recursive on left, right |                                      |
| Radix/ bucket sort | $O(kn)$                          | $O(n+2^d)$  | Sort integer one digit a pass                                | Good for numbers with limited digits |

#### Insertion Sort

```pseudocode
i = 1
while i<length(A)
	x = A[i]
	j = i-1
	while j>=0 and A[j]>x
		A[j+1] = A[j]
		j--
	A[j+1] = x
    i++
```



#### Merge Sort

+ Recursive Top Down

```java
    void mergeSortRecursive(int[] array)
    {
        int length = array.length;
        if(length<=1) return;
        int mid = length/2;
        int[] left = new int[mid];
        int[] right = new int[length-mid];
        for(int i=0; i<mid; i++) left[i] = array[i];
        for(int j=0; j<length-mid; j++) right[j] = array[mid+j];
        mergeSortRecursive(left);
        mergeSortRecursive(right);
        merge(array, left, right);
    }
    void merge(int[] array, int[] left, int[] right){
        int i = 0, j = 0, k=0;
        while(i<left.length && j<right.length){
            if(left[i]<right[j]){
                array[k++] = left[i++];
            }else {
                array[k++] = right[j++];
            }
        }
        while(i<left.length) array[k++] = left[i++];
        while (j<right.length) array[k++] = right[j++];
    }
```

+ Iterative bottom up: pair of 2, pair of 4, ....

  ```java
      void mergeSortIterative(int[] array){
          int length = array.length;
          if(length<=1) return;
          int pairSize = 2;
          while (pairSize<length){
              int i = 0;
              while(i+pairSize<length){
                  mergeIterative(array,i, i+pairSize/2, i+pairSize);
                  i += pairSize;
              }
              //merge remaining part that does not have full pair size to the last full pair. this requires special middle index
              mergeIterative(array,i-pairSize, i,length);
  
              pairSize*=2;
          }
      }
      void mergeIterative(int[] array, int l,int m, int r){
          int[] left = new int[m-l];
          int[] right = new int[r-m];
  
          for(int i=0; i<m-l; i++) left[i] = array[l+i];
          for(int j=0; j<r-m; j++) right[j] = array[m+j];
          int i = 0, j = 0, k=l;
          while(i<m-l && j<r-m){
              if(left[i]<right[j]){
                  array[k++] = left[i++];
              }else {
                  array[k++] = right[j++];
              }
          }
          while(i<left.length) array[k++] = left[i++];
          while (j<right.length) array[k++] = right[j++];
      }
  ```

#### Quick Sort

```java
    void quickSort(int[] array, int left, int right) {
        int pivot = partition(array, left, right);
        if (left<pivot-1) quickSort(array, left, pivot-1);
        if (pivot<right) quickSort(array,pivot,right);
    }
    int partition(int[] array, int left, int right){
        int pivot = array[(left+right) / 2];
        while (left<=right){
            while (array[left]<pivot) left++;
            while (array[right]>pivot) right--;
            if (left <= right){
                swap(array, left, right);
                left++;
                right--;
            }
        }
        return left;
    }
    void swap(int[] array, int left, int right){
        int temp = array[left];
        array[left] = array[right];
        array[right] = temp;
    }
```

#### Radix Sort

[Reference](https://www.geeksforgeeks.org/radix-sort/)

```java
    void radixSort(int[] array) {
        int length = array.length;
        int max = getMax(array, length);
        for (int digit = 1; max/ digit>0; digit*=10){
            countSort(array,length,  digit);
        }
    }
    int getMax(int[] array, int length){
        int max = array[0];
        for (int i=1; i<length; i++){
            if (array[i]>max) max = array[i];
        }
        return max;
    }
    void countSort(int[] array, int length, int digit){
        int[] output = new int[length];
        int i;
        int[] count = new int[10];
        Arrays.fill(count,0);
        for (i=0; i<length; i++) count[(array[i]/digit)%10]++;
        for (i=1; i<10; i++) count[i]+= count[i-1];
        for (i=length-1; i>=0; i--){
            output[count[(array[i]/digit)%10]-1] = array[i];
            count[(array[i]/digit)%10]--;
        }
        for (i=0; i<length; i++) array[i] = output[i];
    }
```

+ Tips: `Arrays.fill(array, value)`



### Searching Algorithms

#### Binary Search

```java
    int binarySearchIterative(int[] array, int x) {
        int left = 0;
        int right = array.length-1;
        int mid;
        while(left<=right){
            mid = (left+right)/2;
            if(array[mid]<x){
                left=mid+1;
            }else if(array[mid]>x){
                right = mid-1;
            }else {
                return mid;
            }
        }
        return -1;
    }

    int binarySearchRecursive(int[] array, int x, int left, int right){
        if (left>right) return -1;
        int mid = (left+right)/2;
        if(array[mid]<x){
            return binarySearchRecursive(array,x,mid+1,right);
        }else if(array[mid]>x){
            return binarySearchRecursive(array, x, left, mid-1);
        }else {
            return mid;
        }
    }

```



p158

## Interview Questions

11 in total, p408

### 10.1 Sorted Merge

You are given two sorted arrays, A and B, where A has a large enough buffer at the
end to hold B. Write a method to merge B into A in sorted order.

+ Draw Example

  + A[1,3,4,6,7,8,9,-,-,-,-,-,-,-]
  + B[2,4,5,6,7,9]
  + out [1,2,3,4,4,5,6,6,7,7,8,9,9,-]

+ Brute Force

  + Enough buffer: no extra spaces

+ Optimize + Conceptual algorithm walk through

  + To prevent moving all elements backwards when inserting into A, start from the right

+ Implement + Test

  + ```java
    void merge(int[] a, int[] b, int Asize, int Bsize){
        int indexA = Asize-1;
        int indexB = Bsize-1;
        int indexMerged = Asize+Bsize-1;
        while(indexB>=0){
            if(indexA>=0 && a[indexA]>b[indexB]){
                a[indexMerged--] = a[indexA--];
            }else{
                a[indexMerged--] = b[indexB--];
            }
        }
    }
    ```

+ Tips

  + To prevent moving all elements backwards when inserting into left, start from the right (vacancy in array)



### 10.2 Group Anagrams

Write a method to sort an array of strings so that all the anagrams are next to each other. *Anagrams* are words or phrases you spell by rearranging the letters of another word or phrase.

+ Draw Example

+ Brute Force

  + Criteria for anagrams: counts for each character are the same.
  + Only need to put together, no need to rank
  + Equivalent to grouping together arrays/ maps
  + $O(n^2)$: compare each pair

+ Optimize + Conceptual algorithm walk through

  + Create comparator class, and use `Array.sort(array, new AnagramComparator())` to sort in $O(n\log n)$ time. 
  + No need to sort full array: map word to the following anagrams. Then go through list of these maps to put back to array as output. 
  + New data structure: HashMapList

+ Implement + Test

  + ```java
    String sortChars(String s){
        char[] sorted = s.toCharArray();
        Arrays.sort(sorted);
        return new String(sorted);
    }
    void sort(String[] array){
        HashMapList<String,String> mapList = new HashMapList<String,String>();
        for(String s: array){
            String sorted = sortChars(s);
            mapList.put(sorted, s);
        }
        int index = 0;
        for(String key: mapList.keySet()){
            ArrayList<String> list = mapList.get(key);
            for(String s: list){
                array[index++]=t;
            }
        }
    }
    ```

  + 

+ Tips

  + Group objects: 

    + convert objects to feature. 
    + create list of HashMapList<feature, object> mapList
    + Go over list of maps to create output

  + feature for anagrams: sorted(s)

  + Create class of map to list: `HashMap<T, ArrayList<E>> map = new HashMap<T, ArrayList<E>>();`

  + Sort chars of string:

    ```java
    String sortChars(String s){
        char[] sorted = s.toCharArray();
        Arrays.sort(sorted);
        return new String(sorted);
    }
    ```



### 10.3 Search in Rotated Array

Given a sorted array of n integers that has been rotated an unknown
number of times, write code to find an element in the array. You may assume that the array was originally sorted in increasing order.

+ Draw Example

  + 5 in {15, 16, 19, 20, 25, 1, 3,4,5,7,10, 14}: output 8

+ Brute Force

  + Scan: $O(n)$, want to go below $O(n)$ , make use of the sorted property

+ Optimize + Conceptual algorithm walk through $O(\log n)$

  + Fast way to find the 'edge' , ie. jump from 25 to 1
    + ~ binary search, start from left, right, middle
    + if left>middle<right, then right = middle
    + go until `right-middle==1 ` then `right` is the starting index
  + Convert index
    + start=0->5, 
    + (before_rotate_n+start)%length = after_rotate_n
  + Can also combine the two into one pass. But the code is messier

+ Implement + Test

  ```java
      int idxBefore2After(int n, int start, int length){
          return (n+start)%length;
      }
      int findStart(int[] arr){
          int left = 0;
          int right = arr.length-1;
          int mid;
          while(left<right-1){
              mid = (left+right)/2;
              if(arr[left]<=arr[right]) return left;
              if(arr[left]>arr[mid] && arr[right]>arr[mid]){
                  right = mid;
              }else if(arr[left]<arr[mid] && arr[right]<arr[mid]){
                  left = mid;
              }
          }
          return right;
      }
      int search(int[] array, int target) {
          int start = findStart(array);
          int length = array.length;
          int left = 0;
          int right = length-1;
          int mid, midAfter, midVal;
          while (left<right){
              mid = (left+right)/2;
              midAfter = idxBefore2After(mid, start,length);
              midVal = array[midAfter];
              if(midVal==target) return midAfter;
              if(midVal>target){
                  right = mid;
              }else {
                  left = mid;
              }
          }
          return -1;
      }
  ```

+ Tips



### 10.4 Sorted Search No Size

You are given an array-like data structure List y which lacks a size method. It does, however, have an element At (i) method that returns the element at index i in 0(1) time. If i is beyond the bounds of the data structure, it returns - 1. (For this reason, the data structure only supports positive integers.) Given a List y which contains sorted, positive integers, find the index at which an element x occurs. If x occurs multiple times, you may return any index.

+ Draw Example

+ Brute Force

  + Go over the array to find size takes $O(n)$

+ Optimize + Conceptual algorithm walk through

  + Go over array by l = 1,2,4,8...
  + if a[l]==x, return l
  + if a[l]>x or a[l]==-1, search from l/2 to l
  + else: l*=2

+ Implement + Test

  + ```java
        int searchRecursive(int[] arr, int start, int end, int x) {
            int idx = end-1;
            if(idx==start){
                if(arr[idx]==x) return idx;
                return -1;
            }
            if(arr[idx]>x || arr[idx]==-1){
                return binarySearch(arr, end/2, end, x);
            }else{
                return searchRecursive(arr, end,end*2, x);
            }
        }
        int binarySearch(int[] arr, int start, int end, int x){
            int mid;
            int midVal;
            while(start<=end){
                mid = (start+end)/2;
                midVal = arr[mid];
                if(midVal==x) return mid;
                if(midVal>x){
                    end=mid;
                }else {
                    start=mid;
                }
            }
            return -1;
        }
    ```

+ Tips

  + Binary search iterative: `while(start<=end)`



### 10.5 Sparse Search

Given a sorted array of strings that is interspersed with empty strings, write a
method to find the location of a given string.

+ Draw Example

  + Input: ball, {"at","", "", "", "ball","", "", "", "car","", "", "", "dad"}
    Output: 4

+ Brute Force

  + Go over array takes $O(n)$ time

+ Optimize + Conceptual algorithm walk through

  + Start, middle, end. If middle is empty, go down until find non-empty string
  + The remaining is the same as binary search
  + Modify string into int for convenience (can easily write a function to convert)
    + {-1,1,-1,-1,-1,-1,2,-1,-1,-1,5,-1,-1,-1,8,-1,-1}

+ Implement + Test

  ```java
      int sparseSearch(int[] arr, int x )
      {
          if(arr.length==0) return -1;
          int start = 0;
          int end = arr.length-1;
          int mid;
          while(start<=end){
              mid = (start+end)/2;
              if(arr[mid]==x) return mid;
              if(arr[mid]==-1){
                  while (arr[mid]==-1 && mid>start) mid--;
              }
              if(arr[mid]!=-1){
                  if(arr[mid]>x){
                      end=mid;
                  }else if(arr[mid]<x){
                      start=mid;
                  }else {
                      return mid;
                  }
              }else{
                  start = (start+end)/2;
              }
          }
          return -1;
      }
  ```

  

+ Tips

### 10.6 Sort Big File

+ Draw Example
  + Imagine you have a 20 GB file with one string per line. Explain how you would sort the file
+ Brute Force
  + Put all strings into memory would be too big
+ Optimize + Conceptual algorithm walk through
  + **External sort:**
    + Load part of the files in memory (size x) each time
    + Afterwards merge them one by one
+ Implement + Test
+ Tips

### 10.7 Missing Int

Given an input file with four billion non-negative integers, provide an algorithm to
generate an integer that is not contained in the file. Assume you have 1 GB of memory available for this task.

+ Draw Example
  + Simply means to search within large number of integers
  + There are totally $2^{32}$ integers (4 billion), and $2^{31}$ non-negative integers (<4 billion). Thus Input file has duplicates. 
  + 1GB  = 8 billion bits > 4 billion, can map each integer to a distinct memory location (bit vector, BV)
  
+ Brute Force

+ Optimize + Conceptual algorithm walk through
  
  + Scan BV until the first index with has value of 0
  + If only 10MB memory and all values are distinct, take in by blocks of ie. 0-1k. Count numbers of integers in that range, if the total number is <1000, then missing value must be in that range
  
+ Implement + Test

  + ```java
    long numberOfInts((long) Integer.MAX_VALUE) +1;
    byte[] bitfield = new byte[(int)(numberOfInts/8)];
    void findOpenNumber() throws FileNotFoundException{
        Scanner in = new Scanner(new FileReader(filename));
        while (in.hasNextInt()){
            int n = in.nextInt();
            bitfield[n/8] |= 1<<(n%8);
        }
        for(int i=0; i<bitfield.length;i++){
            for(int j=0;j<8;j++){
                if((bitfield[i] & (1<<j))==0){
                    System.out.println(i*8+j);
                    return;
                }
            }
        }
        
    }
    ```

  + 

+ Tips

### 10.8 Find Duplicates

You have an array with all the numbers from 1 to N, where N is at most 32,000. The
array may have duplicate entries and you do not know what N is. With only 4 kilobytes ($4*8*2^{10}$ bits) of memory available, how would you print all duplicate elements in the array?

+ Draw Example

  + Create bit vector (BitSet) of 32000 bits

+ Brute Force

+ Optimize + Conceptual algorithm walk through

+ Implement + Test

  + ```java
    class BitSet{
        int[] bitset;
        public BitSet(int size){
            bitset = new int[(size>>5)+1];//divide by 32
        }
        boolean get(int pos){
            int wordNumber = (pos>>5); //divide by 32
            int bitNumber = (pos & 0x1F); // mod 32
            return (bitset[wordNumber] & (1<<bitNumber))!=0;
        }
        void set(int pos){
            int wordNumber = (pos>>5); //divide by 32
            int bitNumber = (pos & 0x1F); // mod 32
            bitset[wordNumber] |= 1<<bitNumber; 
        }
    }
    
    void checkDuplicates(int[] array){
        BitSet bs = new BitSet(32000);
        for(int i = 0; i< array.length; i++){
            int num = array[i];
            int num0 = num-1;
            if(bs.get(num0)){
                System.out.println(num;)
            }else{
                bs.set(num0);
            }
        }
    }
    
    ```

  + 

+ Tips

  + Use BitSet to map integer to bits

    ```java
    int wordNumber = (pos>>5); //divide by 32
    int bitNumber = (pos & 0x1F); // mod 32
    (bitset[wordNumber] & (1<<bitNumber))!=0;// check if integer exists
    bitset[wordNumber] |= 1<<bitNumber; //set integer exists
    ```



419



