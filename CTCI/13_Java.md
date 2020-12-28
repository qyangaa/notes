# Java

### Overloading vs Overriding

+ Overloading: two methods have the same name but differ in type or arguments

  ```java
  public double computeArea(Circle c){...}
  public double computeArea(Square s){...}
  ```

+ Overriding: method shares the same name and function signature as another method in its super class

  ```java
  public abstract class Shape{
      public void printMe(){...}
  }
  public class Circle extends Shape{
      private double rad = 5;
      public void printMe(){...}
  }
  ```

### Collection Framework

+ ArrayList: dynamically resizing array

  ```java
  ArrayList<String> myArr = new ArrayList<String>();
  myArr.add("one");
  System.out.println(myArr.get(0));
  ```

+ Vector: synchronized `add()`,`get()`

+ LinkedList: 

  ```java
  LinkedList<String> myLinledList = new LinkedList<String>();
  myLinkedList.add("two");
  myLinkedList.addFirst("one");
  Iterator<String> iter = myLinkedList.iterator();
  while(iter.hasNext()){
      System.out.println(iter.next());
  }
  ```

+ HashMap

  ```java
  HashMap<String, String> map = new HashMap<String, String>();
  map.put("one","uno");
  System.out.println(map.get("one"));
  ```

  



p177

## Interview Questions

### 13.1 Private Constructor

In terms of inheritance, what is the effect of keeping a constructor private?

+ Only class A's inner class can access its private methods
+ Subclass calls parent's constructor in inheritance
+ A can only be inherited by its own or its parents' inner classes.



### 13.2 Return from Finally

In Java, does the finally block get executed if we insert a return statement inside the try block of a try-catch-finally?

+ try-catch-finally
  + try
  + catch: `catch(BackNumberException e){...}`
  + finally: always be executed, with or without exceptions
+ Finally gets executed when try block exists whatever try block does
+ Finally not executed when:
  + virtual machine exits during try/catch
  + thread is killed



### 13.3 Final, etc.

What is the difference between final, finally, and finalize?

+ final: variable/ method/ class is not changeable
  + primitive variable: value cannot change
  + reference variable: reference cannot point to other object
  + method: cannot be overriden
  + class: cannot be subclasses
+ finally: in try-catch-finally to ensure some code is always executed
+ finalize(): garbage collector once it determines that no more reference exist

### 13.4 Generics vs. Templates

Explain the difference between templates in C++ and generics in Java

+ Generics

  + java: "type erasure", eliminates the parameterized types when source code is translated to JVM byte code

    ```java
    Vector<String> vector = new Vector<String>();
    vector.add(new String("hello")));
    String str = vector.get(0);
    ==>
    Vector vector = new Vector();
    vector.add(new String("hello"));
    String str = (String) vector.get(0);
    ```

  + C++:  compiler creates a new copy of the template code for each type

    + i.e. two instances of `MyClass<Foo>`  share the same static variable, while two instances of `MyClass<Foo>` and `MyClass<Bar>` do not.

+ Summary

  |                                                              | Java | C++  |
  | ------------------------------------------------------------ | ---- | ---- |
  | Instances of same class with different type share static variables (not creating new copy of class) | Yes  | No   |
  | Can use primitive types                                      | No   | Yes  |
  | Can restrict type to be a certain type                       | Yes  | No   |
  | Type parameter can be instantiated                           | No   | Yes  |
  | Type parameter can be used for static methods and variables  | No   | Yes  |
  | all instance are same type                                   | Yes  | No   |

### 13.5 TreeMap, HashMap, LinkedHashMap

|                           | HashMap               | TreeMap         | LinkedHashMap         |
| ------------------------- | --------------------- | --------------- | --------------------- |
| Lookup and insertion time | $O(1)$                | $O(\log N)$     | $O(1)$                |
| Ordered                   | No                    | Insertion order | True order            |
| Implementation            | array of linked lists | Red-black tree  | doubly-linked buckets |

### 13.6 Object Reflection

+ Get reflective information about java classes and objects
+ Operations:
  + Get information
  + Create new instance
  + Get and set object fields directly by getting field reference, regardless of what the access modifier is
+ Benefit
  + help observe or manipulate the runtime behavior of applications
  + help debug or test
  + can call all methods by name when don't know the method in advance

### 13.7 Lambda Expressions

There is a class `Country` that has methods `getContinent ()` and `getPopulation()` . Write a function `int getPopulation(List<Country> countries,String continent)` that computes the total population of a given continent, given a list of all countries and the name of a continent.

```java
int getPopulation(List<Country> countries, String continent){
    Stream<Integer> populations = countries.stream().map( c-> c.getContinent.equals(continent)? c.getPopulation() : 0);
    return populations.reduce(0, (a,b)-> a+b);
}
```

+ Stream:
  + map: 
    `List square = number.stream().map(x->x*x).collect(Collectors.toList());`
  + filter:
    `List result = names.stream().filter(s->s.startsWith("S")).collect(Collectors.toList());`
  + sorted:
    `List result = names.stream().sorted().collect(Collectors.toList());`
  + collect: return result of intermediate operations
  + forEach: iterate
    `number.stream().map(x->x*x).forEach(y->System.out.println(y));`
  + reduce: to a single value
    `int even = number.stream().filter(x->x%2==0).reduce(0,(ans,i)-> ans+i);`

### 13.8 Lambda Random

Using Lambda expressions, write a function `List<Integer>
getRandomSubset (List< Integer> list)` that returns a random subset of arbitrary size. All subsets (including the empty set) should be equally likely to be chosen

+ Iterate over list, pick each item by a random boolean

```java
List<Integer> getRandomSubset(List<Integer> list){
    Random random = new Random();
    List<Integer> subset = list.stream().filter(
    k->{return random.nextBoolean();}).collect(Collectors.toList());
    return subset;
}
```



p453