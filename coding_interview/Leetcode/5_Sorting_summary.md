### Count then sort

```python
import collections
import heapq

count = list(collections.Counter(items).items())
#sort by count (max) then alphabetical
count.sort(key=lambda x: (-x[1], x[0]))
#heap to find top k count (logk time)
heapq.nlargest(k,count, lambda x: (-x[1], x[0]))
```



题分：15

系统评 **10**分

- | **A、** | Selection sort is not stable, insertion sort is stable. |      |
  | ------- | ------------------------------------------------------- | ---- |
  | V       |                                                         |      |

- | **B、** | Merge sort space complexity is higher than heap sort. |      |
  | ------- | ----------------------------------------------------- | ---- |
  | V       |                                                       |      |

- | **C、** | The worst time complexity of merge sort can be O(n^2) |      |
  | ------- | ----------------------------------------------------- | ---- |
  |         |                                                       |      |

- | **D、** | The best time complexity of bubble sort can be O(n) |      |
  | ------- | --------------------------------------------------- | ---- |
  | V       |                                                     |      |

- | **E、** | Radix sort can be O(n) time complexity, which means we should always try to use Radix sort. |      |
  | ------- | ------------------------------------------------------------ | ---- |
  |         |                                                              |      |

- | **F、** | Quick sort can always be better than the other sorting algorithms, that is why it is called quick sort. |      |
  | ------- | ------------------------------------------------------------ | ---- |
  |         |                                                              |      |

题分：15

系统评 **0**分

- | **A、** | (a^b)^(a^c) = b^ c |      |
  | ------- | ------------------ | ---- |
  | V       |                    |      |

- | **B、** | (~1)>>3 + 1>>3 = 0 |      |
  | ------- | ------------------ | ---- |
  |         |                    |      |

- | **C、** | (~1)<<3 < ~(1<<3) |      |
  | ------- | ----------------- | ---- |
  | V       |                   |      |

- | **D、** | ((~1)^3)<<1 = -6 |      |
  | ------- | ---------------- | ---- |
  | V       |                  |      |

- | **E、** | ((-1)^3)>>>1 = -2 |      |
  | ------- | ----------------- | ---- |
  |         |                   |      |





\1.  多选题 |难度：2

Which statements are correct about data structure, algorithms and operating system

题分：5

系统评 **5**分

- | **A、** | For an undirected graph, if we want to get a topology sort, there cannot be a cycle inside. |      |
  | ------- | ------------------------------------------------------------ | ---- |
  |         |                                                              |      |

- | **B、** | A heap does not have to be max heap or min heap, you can make any element at top based on the sorting criteria. |      |
  | ------- | ------------------------------------------------------------ | ---- |
  | V       |                                                              |      |

- | **C、** | Tree is a special graph. Linked List is also a special graph |      |
  | ------- | ------------------------------------------------------------ | ---- |
  | V       |                                                              |      |

- | **D、** | We can use an expression of bit manipulation to present an absolute value of an integer. |      |
  | ------- | ------------------------------------------------------------ | ---- |
  | V       |                                                              |      |

- | **E、** | Integer a = new Integer(126). Integer b = new Integer(126).a == b will return true since 126 is within the range of the constant pool |      |
  | ------- | ------------------------------------------------------------ | ---- |
  |         |                                                              |      |

- | **F、** | Garbage collection will happen automatically for Java but not for C and C++. |      |
  | ------- | ------------------------------------------------------------ | ---- |
  | V       |                                                              |      |

\2.  多选题 |难度：2

About tree, which statements are correct?

题分：5

系统评 **0**分

- | **A、** | If binary tree has N nodes, the height should be O(lgn). |      |
  | ------- | -------------------------------------------------------- | ---- |
  |         |                                                          |      |

- | **B、** | We can use array to store a complete binary tree and it saves more space then using Node. |      |
  | ------- | ------------------------------------------------------------ | ---- |
  | V       |                                                              |      |

- | **C、** | A node in a tree can have 20 children if needed. |      |
  | ------- | ------------------------------------------------ | ---- |
  | V       |                                                  |      |

- | **D、** | Red-Black tree is always a Balanced tree. |      |
  | ------- | ----------------------------------------- | ---- |
  | V       |                                           |      |

- | **E、** | A Binary search tree can guarantee O(lgn) time to find an element |      |
  | ------- | ------------------------------------------------------------ | ---- |
  |         |                                                              |      |

\3.  多选题 |难度：2

Which sort can use constant space to finish sorting?

题分：5

系统评 **0**分

- | **A、** | Bubble Sort |      |
  | ------- | ----------- | ---- |
  | V       |             |      |

- | **B、** | Heap Sort |      |
  | ------- | --------- | ---- |
  | V       |           |      |

- | **C、** | Radix Sort |      |
  | ------- | ---------- | ---- |
  |         |            |      |

- | **D、** | Selection Sort |      |
  | ------- | -------------- | ---- |
  | V       |                |      |

- | **E、** | Merge Sort |      |
  | ------- | ---------- | ---- |
  |         |            |      |

- | **F、** | Insertion Sort |      |
  | ------- | -------------- | ---- |
  | V       |                |      |

\4.  多选题 |难度：2

Which sort can always keep O(nlogn) time complexity

题分：5

系统评 **0**分

- | **A、** | Quick Sort |      |
  | ------- | ---------- | ---- |
  |         |            |      |

- | **B、** | Merge Sort |      |
  | ------- | ---------- | ---- |
  | V       |            |      |

- | **C、** | Bubble Sort |      |
  | ------- | ----------- | ---- |
  |         |             |      |

- | **D、** | Heap Sort |      |
  | ------- | --------- | ---- |
  | V       |           |      |

- | **E、** | Bucket Sort |      |
  | ------- | ----------- | ---- |
  |         |             |      |