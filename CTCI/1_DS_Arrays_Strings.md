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

### Implementation





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

