# Data Types

### Primite types

| Type    | Bytes | Range        | Example               |
| ------- | ----- | ------------ | --------------------- |
| byte    | 1     | `[128,127]`  | `byte age = 30;`      |
| short   | 2     | `[-32K,32K]` | `short age = 30`      |
| int     | 4     | `[-2B,2B]`   | `int n = 123`         |
| long    | 8     |              | `long l = 123L`       |
| float   | 4     |              | `float f = 0.1F`      |
| double  | 8     |              | `double d = 0.1`      |
| char    | 2     | A,B,C..      | `char a = 'a'`        |
| boolean | 1     | true/false   | `boolean isTrue=true` |

### Data Types

- ArrayList<E> 
  - Add elements
    - add(elem), add(idx, elem)
  - Find Object
    - contains(obj), indexOf(obj), lastindexOf(obj)
  - Remove elements
    - remove(idx), remove(obj)
  - Setter \& getter
    - get(idx, elem), get(idx)
  - To Array
    - Object[] toArray();
  - Constructing
    - new ArrayList();
    - new ArrayList(capacity); 
    - new ArrayList(Collection); 
    - Arrays.asList(T... a); List $<$ String $>$ stooges = Arrays.asList("Larry", "Moe", "Curly");
- LinkedList $<E>$: Queue, Stack
  - Add
    - add(e) addFirst $(e),$ addLast $(e)$
      offer(e), offerFirst(e), offerLast(e)
    - push(e)
  - Retrieve
    - get(i), getFirst(), getLast() - - no removal 
    - peek(), peekFirst(), peekLast() - - - no removal
    - poll(), pollFirst(), pollLast()
    - pop()
- HashMap $<K, V>$
  - Contains
    - containsKey(key), containsValue(value)
  - Add
    - put(key, value), putAll(Map)
  - Get
    - get(key)
  - Remove
    - remove(key) Iteration
      entrySet(), keySet()
- HashSet
  - Contains
    - contains(obj)
  - Add
    - add(elem)
  - Remove
    - remove(obj)
- String
  - Non-static functions $\quad$ final class String
    - charAt(index) 
    - concat(string)
    - length()
    - substring(int beginindex, int opt_endindex)
    - toCharArray()
    - equals ()$,$ equalsignoreCase() 
    - toLowercase(), toUppercase() 
    - split(separator)
  - Static functions
  - String.valueOf();
  - Other functions
    - toString() 
    - Integer.parselnt(string), Double.parseDouble(string), etc.
  - StringBuilder 
    - append(..) insert(...)
    - setCharAt(index, ch), deleteCharAt(index, ch) reverse()  toString()

### Shared APIs

- void clear();
- boolean isEmpty(); 
- int $\operatorname{size}()$
- boolean addAll(Collection); 
- boolean removeAll(Collection);