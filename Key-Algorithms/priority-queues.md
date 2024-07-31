title: Key Algorithms - Priority Queues

#Priority Queues

While researching the article on [Vector Search Trees]({% link Key-Algorithms/vector-search-trees.md %}), I found that two methods for constructing ball trees and the algorithm for querying ANNOY both involved *Priority Queues*. Since these are an important component of a number of different algorithms, it is worth examining them in detail.

Suppose we want to iterate over a set of items in a particular order. The naive way of doing this is to sort the list of items and then iterate over them. However, sorting is an expensive operation for large datasets, and we may want to add further items to the list while still iterating, which would necessitate re-sorting the list each time. We therefore need a more efficient way of tackling this.

Priority Queues address this by storing the data in a partially ordered data structure whose elements can be reordered efficiently when items are added or removed. Most implementations use a *heap*, which is a list of items with the following properties.

1. The item at index $i$ is the parent of the items at $2i+1$ and $2(i+1)$
2. The parent is less than or equal to each of its children.

These properties can be efficiently maintained by the following operations.

*Sift Up*
: While an item is less than its parent (or not at the start of the list), swap it with its parent and check to see if its less than its new parent

*Sift Down*
: While an item is greater than the smaller of its two children (or not at the end of the list), swap it with that child and check to see if it is greater than either of its new children.

(*Note*: What I'm describing here is a *Min Heap*, which is used when we want to iterate over our items in ascending order. Most Python implementations of priority queues use this. There are also *Max Heaps*, which are used to iterate over items in descending order).

To add an item to the heap, we place it at the end, and then Sift Up until it reaches its proper place. When we remove the first item from the heap during iteration, we more the last item from the heap to the first position, and then Sift Down until it reaches its proper place.

There are several implementations of priority queues in Python [heapq](https://docs.python.org/3/library/heapq.html) in the standard library, [heapdict](https://pypi.org/project/HeapDict/) which implements a dictionary interface and allows the priority of items to be altered, and [PriorityQueue](https://docs.python.org/3/library/queue.html#queue.PriorityQueue) in the standard Queue library, which is useful for sheduling data items to be processed by workers in a multithreaded application.

Prioritising tasks is an important part of many algorithms, so this is a useful tool to be aware of when designing an algorithm.

by [Dr Peter J Bleackley]({% link index.md %})

[Key Algorithms]({% link Key-Algorithms/index.md %})

Search and Navigation
: [Vector Search Trees]({% link Key-Algorithms/vector-search-trees.md})
: *Priority Queues*
: [Graph Search Algorithms]({% link Key-Algorithms/graph-search-algorithms.md %})
