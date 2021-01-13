### Heap:

+ implemented in aray. 
  + parent -> children: n-> 2n+1, 2n+2
  + child -> parent: n->(n-1)/2







### Python Heapq

+ Only supports minHeap
+ Use `-value` for maxheap. 

```python
import heapq
list=[]
heap = heapq.heapify(list)
heapq.heappush(heap, item)
item = heapq.heappop()
size=len(heap)
peakItem = heap[0]
```





