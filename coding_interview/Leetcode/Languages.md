

|                         | C++                                  | Java                                               | Python    | JS   |
| ----------------------- | ------------------------------------ | -------------------------------------------------- | --------- | ---- |
| Array                   | `int a[5]`                           | `int[] a=new int[5]`                               |           |      |
| 2D Array                | `char c[3][6]`                       | `char[][] c=new char[3][], arr[0]=new int[3]`      |           |      |
| Array length            | Cannot get length                    | `a.length`                                         |           |      |
| 2D Array Implementation | Fixed length continuous              | Pointer to another array, length in object header  |           |      |
| Mutable Array           | `vector<int> a`                      | `ArrayList<Integer> a = new ArrayList<Integer>(5)` |           |      |
| Sort                    |                                      | `Arrays.sort(A)`                                   |           |      |
| Swap                    |                                      | `int temp=a; a=b;b=temp`                           | `a,b=b,a` |      |
| Stack                   | `stack<string> stack`                | `Stack<String> stack = new Stack<>()`              |           |      |
| Queue                   | `ququq<string> queue`                | `Queue<String> queue = new LinkedList<>()`         |           |      |
| HashMap                 | `unordered_map<string, double> map;` | `Map<Object,Object> map = new HashMap<>()`         |           |      |
|                         |                                      |                                                    |           |      |
|                         |                                      |                                                    |           |      |
|                         |                                      |                                                    |           |      |
|                         |                                      |                                                    |           |      |
|                         |                                      |                                                    |           |      |
|                         |                                      |                                                    |           |      |
|                         |                                      |                                                    |           |      |
|                         |                                      |                                                    |           |      |
|                         |                                      |                                                    |           |      |
|                         |                                      |                                                    |           |      |



## Java

### Types

+ Primitive

  + only 8: btye, char, short, int, long, float, double, boolean
  + store in stack memory (consequtive)
  + `"="`copy value

+ Reference Type:

  + initialize: `Type a = new Type()`

  + store in heap memory (address in stack memory)

  + `"="`copy address, point to same instance. `new` create new instance

  + Initialize with primitive assigment `String a = "abc"`

    + Create same object if value match

      ```java
      String a="abc";
      String b="abc";
      a==b;//true
      String c= new String("abc");
      a==c;//false
      a.equals(c);//true
      ```

    + Same for small integers, create new reference when large

      ```java
      Integer a=1;
      Integer b=1;
      a==b;//true
      Integer c=1000;
      Integer d=1000;
      c=d;//false
      ```


### Memory

+ Stack: fast, for primitie variables, function calls
+ Heap: slow, reference variables, dynamically allocate space

### LinkedList

+ many operations

```java
LinkedList<String> llistobj  = new LinkedList<String>();
llistobj.add("Hello");
llistobj.add(2, "bye"); //idx
llistobj.addAll(arraylist);
llistobj.add(5, arraylist); //start idx
llistobj.addFirst("text"); //left
llistobj.addLast("Chaitanya"); //right
llistobj.clear();
Object str= llistobj.clone();//copy
boolean var = llistobj.contains("TestString");
Object var = llistobj.get(2); //idx
Object var = llistobj.getFirst();
Object var= llistobj.getLast();
llistobj.indexOf("bye");
int pos = llistobj.lastIndexOf("hello");
Object o = llistobj.poll();//pop first (left) and return
Object o = llistobj.pollFirst();
Object o = llistobj.pollLast();
llistobj.remove(); //pop only
llistobj.remove("Test Item");
llistobj.removeFirst();
llistobj.removeLast();
llistobj.removeFirstOccurrence("text");
llistobj.removeLastOccurrence("String1);
llistobj.set(2, "Test");
llistobj.size();
```

### Comparison

+ .equals(..) for reference types
+ `==` for primitve types only