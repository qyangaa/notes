p100

## Hash Tables

### Basics

+ **Generation**: compute ```index=hash(key)``` , then store key-value pair at this index (to avoid collision) with:
  + linked list:  $O(N)$ worst, $O(1)$ when collision is minimized
  + Binary search tree: $O(\log N)$ , but less space allocation
+ **Hashmap vs. Hashtable:**

|                                 | Hashmap    | Hashtable     |
| ------------------------------- | ---------- | ------------- |
| synchronized/ thread-save       | No         | Yes           |
| # null key/ value allowed       | 1/multiple | 0/0           |
| When synchronization not needed | Preferred  | Not preferred |

|                          | Hashmap                       | Hashset                                       |
| ------------------------ | ----------------------------- | --------------------------------------------- |
| internal working         | {“Hello”, “Hi”, “Bye”, “Run”} | {1->”Hello”, 2->”Hi”, 3->”Bye”, 4->”Run”}     |
| duplicate values allowed | yes                           | no                                            |
| null allowed             | single null value             | single null key and any number of null values |



### Implementation

+ Java HashMap: initial capacity = 16; default load factor =0.75f, threshold = initial capacity*load factor = 12
+ Internal working: Node(int hash, K key, V value, Node next)
+ [Implementation of hashtable with linear probing:](https://www.hackerearth.com/practice/data-structures/hash-tables/basics-of-hash-tables/tutorial/)

```c++
string HashTable[21];
int hashTableSize = 21;

void insert(string s){
	int index = hashFunc(s);
    while(hashTable[Index]!="")
        index=(index+1)%hashTableSize;
    hashTable[index]=s;
}
void search(string s){
    int index = hashFun(s);
    while(hashTable[index]!=s and hashTable[index]!="")
        index = (index+1)%hashTableSize;
    if (hashTable[index]==s)
        cout << s << " is found!" << endl;
    else
        cout << s << " is not found!" << endl;
}

```





## ArrayList & Resizable Arrays

### Basics

+ ArrayList: array with dynamic resizing with $O(1)$ access time. 
+ Resizing mechanism: double in size when full ($O(N)$ time), but insertion time amortized to $O(1)$ , because $N / 2+N / 4+N / 8+\cdots+2+1<N$

### Examples

``` java
ArrayList<string> merge(string[] words, string[] more){      		ArrayList<string> sentence = new ArrayList<string>(); 
    for (String w : words) sentence.add(w); 
    for (String w : more) sentence.add(w); 
    return sentence;
}
```

### Implementation

[Ref](https://www.geeksforgeeks.org/how-to-implement-our-own-dynamic-array-class-in-java/)

```java
import java.util.*;
class ArrayListClass{
    private int arr[];
    private int capacity;
    private int current;//current size
    public ArrayListClass(){//constructor
        arr = new int[1];
        capacity = 1;
        current = 0;        
    }
    public void push(int data){//append to end
        if (current==capacity){
            int temp = new int[2*capacity];//doubling array capacity
            for (int i=0;i<capacity; i++)
                temp[i] = arr[i];
            capacity*=2;//doubling capacity
            arr=temp;
        }
        arr[current]=data;
        current++;
    }
   	void push(int data, int index){
        if (index==capacity)
            push(data);
        else
            arr[index] = data;
    }
    int get(int index){
        if(index<current)
            return arr[index];
        return -1;
        
    }
    int pop(){
        current --;        
    }
    int size(){
        return current;
    }
    int getcapacity(){
        return capacity;
    }
    void print(){
        for(int i = 0; i<current; i++){
            System.out.print(arr[i]+" ");
        }
    }
        
}
```



## StringBuilder

### Basics

```java
String joinWords(String[] words) {
    String sentence = "";
    for (String w : words) {
    sentence = sentence + w;
    return sentence;
}
```

+ A new copy of string is created at every iteration: $1x+2x+3x+...+nx = O(xn^2)$

```java
String joinWords(String[] words) {
    StringBuilder sentence = new StringBuilder();
    for (String w : words) {
        sentence.append(w);
    }
    return sentence.toString();
}
```

+ StringBuilder creates a resizable array of all the strings, copying them back to a string only when necessary, to reduce number of string objects created

### Implementation

<a href="https://www.thecodingdelight.com/java-stringbuilder-class/#ftoc-heading-4">reference</a>

```java
private static final int DEFAULT_BUFFER_SIZE=16;
private static final int DEFAULT_BUFFER_SIZE=2;

private char[] str;
private int size;
private int charCount;
    
public StringBuilder(){// Default Constructor
    this.size = DEFAULT_BUFFER_SIZE;
    this.str = new char[DEFAULT_BUFFER_SIZE]
}
public StringBuilder append(String str){
    while (resizeRequired(str)){
        resizeBuffer(str);
    }
    addString(str);
    updateCharCount(str.length());
    return this;
}
private boolean resizeRequired(String newInput){
    return this.charCount+newInput.length()>this.size;
}

//resize array if overflows
private void resizeBuffer(String newInput){
    int oldSize = this.size;
    this.size *= BUFFER_MULTIPLIER;
    char[] newStr = new char[this.size];//convert string to array
    System.arraycopy(this.str, 0, newStr, 0, oldSize);
    this.str = newStr;    
}
private void addString(String str){
    System.arraycopy(str.toCharArray(),0,this.str, this.charCount, str.length());
}
private void updateCharCount(int charCount){
    this.charCount += charCount;
}
```

## Interview Questions

### 1.1 Is Unique

p204

 Implement an algorithm to determine if a string has all unique characters. What if you cannot use additional data structures?

+ Draw Example
  + "mooncake"-> True
  + "abcedf"-> False

+ Brute Force
  + "a": check "bcedf", "b": check "cedf".... $(n-1)+(n-2)+...+1 = O(n^2)$

+ Optimize + Conceptual algorithm walk through
  + Sort and compare neighbors $O(n\log n)$
  + Data structure: Set. traverse $O(n)$, check if in set $O(1)$, if not put in set, if in set return True. Total $O(n)$
  + If no additional data structure allowed, use something like a hashtable. Use an array of size of total number of characters (ASCII coding, $O(1)$ space for English). Put in array if corresponding index not filled, return True if filled. $O(n)$ time, but may be faster than set.

+ Implement + Test

  + ```java
    public class Unique_1_1 {
        public boolean answer(String s){
            int[] asciiArray = new int[127];
            for (int i=0; i<s.length(); i++){
                if (asciiArray[(int)s.charAt(i)]!=0){
                    return true;
                }else{
                    asciiArray[(int)s.charAt(i)] = 1;
                }
            }
            return false;
        }
        public void test(){
            System.out.println(this.answer("abcdef"));
            System.out.println(this.answer(" 123 456"));
            System.out.println(this.answer(""));
            System.out.println(this.answer("mooncake"));
        }
    }
    ```

+ Answer: use bit vector to reduce space usage (space of a single int)

  + ```java
    public class QuestionB {
    
    	/* Assumes only letters a through z. */
    	public static boolean isUniqueChars(String str) {
    		if (str.length() > 26) { // Only 26 characters, >26 guarantees repetition
    			return false;
    		}
    		int checker = 0;
    		for (int i = 0; i < str.length(); i++) {
    			int val = str.charAt(i) - 'a';
    			if ((checker & (1 << val)) > 0) return false;
    			checker |= (1 << val);
    		}
    		return true;
    	}
    	
    	public static void main(String[] args) {
    		String[] words = {"abcde", "hello", "apple", "kite", "padle"};
    		for (String word : words) {
    			System.out.println(word + ": " + isUniqueChars(word));
    		}
    	}
    
    }
    ```

  + 

### 1.2 Check Permutation

p205

Given two strings, write a method to decide if one is a permutation of the other.

+ Draw Example
  + "abcde", "bceda" true
  + "aabbcc", "abcba" false
  + "aabbcc", "abcabc" true
  + "ababab","bababb" false

+ Brute Force
  + Go over one string, list all permutations, and see if the other string is in the permutation. $O(n!)$

+ Optimize + Conceptual algorithm walk through
  + Simplify: equivalent to whether the two strings have same character composition
  + If not the same length: false
  + sort: sort, then traverse at the same time $O(n\log n)$
  + hash map: check occurrence of each character, need to traverse each string once $O(n)$
  + Answer: 
    + no need for hash map. Similar to Q1, we can use an array if length 128 for ASCII.
    + if not same count for same length, the second string must have >1 character whose count is larger than that of the first string

+ Implement+Test

  + ```java
    public class Permutation_1_2 {
        public boolean answer(String s, String t) {
            if (s.length() != t.length()) return false; // Permutations must be same length
            int[] letters = new int[128]; // Assumption: ASCII
            for (int i = 0; i < s.length(); i++) {
                letters[s.charAt(i)]++;
            }
            for (int i = 0; i < t.length(); i++) {
                letters[t.charAt(i)]--;
                if (letters[t.charAt(i)] < 0) {
                    return false;
                }
            }
            //if not same count for same length, t must have <0 char.
            return true;
        }
        public void test(){
            String[] first = {"abcde", "hello", "apple", "kite", "padle"};
            String[] second = {"abdce", "helo", "apppl", "ktie", "padlw"};
            for (int i=0; i<first.length; i++) {
                System.out.println(first[i]+","+ second[i] + ": " + answer(first[i],second[i]));
            }
        }
    }
    ```

    

### 1.4 Palindrome Permutation

p207

Given a string, write a function to check if it is a permutation of a palindrome. A palindrome is a word or phrase that is the same forwards and backwards. A permutation is a rearrangement of letters. The palindrome does not need to be limited to just
dictionary words.

+ Draw Example
  + "TactCoa" -> true :"tacocat'; "atcocta '; etc."
  + Seems that space and capital don't matter, can ignore space all together

+ Brute Force
  + Write all permutation and see if has palindrom

+ Optimize + Conceptual algorithm walk through
  + Palindrome permutation iff: 
    + count = even: all occurrences are even
    + count = odd: all but one occurrences are even
  + Then simply count number of occurrences using ASCII table as we did in 1.1

+ Implement + Test

  ```java
  public boolean answer(String s){
      s = s.toLowerCase();
      int oddCount = -s.length()%2; //if even, no odd allowed, if odd, allow 1
      int[] asciiArray = new int[128];
      for(int i=0; i<s.length(); i++){
          if (asciiArray[(int)s.charAt(i)]!=0){
              asciiArray[(int)s.charAt(i)]=0;
          }else{
              asciiArray[(int)s.charAt(i)]=1;
          }
      }
      for(int i=0; i<asciiArray.length; i++){
      	if (asciiArray[i]==1){
              oddCount++;
          }
          if(oddCount>0){
              return false;
          }
      }
      return true;
  }
  ```

  

### 1.5 One Edit Away

There are three types of edits that can be performed on strings: insert a character, remove a character, or replace a character. Given two strings, write a function to check if they are one edit (or zero edits) away.

+ Draw Example
  + pale,ple -) true
    pales, pale -) true
    pale, bale -) true
    pale, bae -) false

+ Brute Force
  + Check all possible edits

+ Optimize + Conceptual algorithm walk through
  + If length same: must be replace. Traverse and see if only one char is different
  + If length different: [insert and remove are symmetric] we must remove one char from the shorter string or insert one to the longer string
  + Get lengths first, 
    + If length same: traverse together and return if !differentCounts>1
    + if length differ by 1, traverse together, and remove from longer string if differ. return if !removeCounts>1

+ Implement + Test

  + ```java
    public boolean oneSubstitutionAway(String s1, String s2){
        int diffCount = 0;
        for(i=0;i<s1.length(); i++){
            if(s1.charAt(i)!=s2.charAt(i)){
                diffCount++;
               	if(diffCount>1){
                    return false
                }
            }
        }
        return true
    }
    public boolean oneRemoveAway(String s1, String s2){
        int rmvCount = 0;
        int i=0;
        int j=0;
        while(i<s1.length){
            if(s1.charAt(i)!=s2.charAt(j)){
                rmvCount++;
                if(rmvCount>1){
                    return false;
                }
                i++;
            }
            i++;
            j++;
        }
        return true;
    }
    public boolean answer(String s1, String s2){
        int len1 = s1.length();
        int len2 = s2.length();
        if ((len1-len2)>1||(len1-len2)<-1){
            return false;
        }
        if((len1-len2)==0){
            return oneSubstitutionAway(s1,s2);
        }
        if((len1-len2)==1){
            return oneRemoveAway(s1,s2);
        }else{
            return oneRemoveAway(s2,s1);
        }
    }
    ```

  + Improve: combine oneRmoveAway and oneReplaceAway

    ```java
    public boolean oneEditAway(String s1, String s2, Boolean rmv){
        int editCount = 0;
        int i=0;
        int j=0;
        while(i<s1.length){
            if(s1.charAt(i)!=s2.charAt(j)){
                if(rmv){rmvCount++;}            
                if(editCount>1){
                    return false;
                }
                i++;
            }
            i++;
            j++;
        }
        return true;
    }
    ```

    

### 1.6 String Compression

Implement a method to perform basic string compression using the counts of repeated characters. For example, the string aabcccccaaa would become a2blc5a3 . If the "compressed" string would not become smaller than the original string, your method should return the original string. You can assume the string has only uppercase and lowercase letters (a - z).

+ Draw Example

  + ```
    String[] input = {"aabcccccaaa", "hello", "kite", "paaadlle"};
    String[] output = {"a2blc5a3","hello","kite","pa3dl2e"}
    ```

+ Brute Force
  + traverse string, if same, keep count, if not, concat count, concat new char, and start new count
  + This is slow because ==string concat operates in $O(n^2)$.==

+ Optimize + Conceptual algorithm walk through
  + Use string builder to reduce concat to amortized $O(n)$

+ Implement + Test

  + ```java
        public String answer(String s){
    		StringBuilder compressed = new StringBuilder();
            int countConsecutive = 0;
            for(i=0;i<s.ength();i++){
                countConsecutive++;
                if(s.charAt(i)!=s.charAt(i+1)||i+1>=s.length()){
                    compressed.append(str.charAt(i));
                    compressed.append(countConsecutive);
                    countConsecutive=0;
                }      
            }
            return compressed.length < s.length()? compressed.toString(): str;
        }
    ```

### 1.9 String Rotation

Assume you have a method isSubstring which checks if one word is a substring
of another. Given two strings, s1 and s2, write code to check if s2 is a rotation of s1 using only one call to isSubstring (e.g., "waterbottle" is a rotation of "erbottlewat").

Adding assumptions:

Assume isSubstring runs in $O(n)$

+ Draw Example
  + s1 = x+y, s2 = y+x, x = s1[:k], y=s1[k:]

+ Brute Force
  + check every rotation of s1 and see if s2 is one of them. $n$ rotation and $n$ bit comparison in each rotation $O(n^2)$

+ Optimize + Conceptual algorithm walk through

  +  If s1 and s2 have same length, just check if s2 (yx) is a substring of s1+s1(xyxy), takes $O(n)$ time.

+ Implement

  + ```java
    public boolean answer(string s1, String s2){
       	int len = s1.length();
        if (len ==s2.length() && len>0){
            String s1s1 = s1+s1;
            return isSubstring(s1s1, s2);
        }
        return false
    }
    ```

  + 



