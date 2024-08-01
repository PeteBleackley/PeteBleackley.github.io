---
layout: default
title: Key Algorithms - Graph Search Algorithms
---

# Graph Search Algorithms

There are many applications in which we might wish to navigate our way through a graph of nodes and edges - route finding for satnav systems, controlling character movement in video games, routing traffic on the Internet, crawling the web for a search engine, or finding information in a vector database, for example. 

Algorithms used for this task make use of a *frontier*, which initally contains a node representing the starting point of our search. At each stage of the search, we retrieve an node from the frontier, and examine the nodes it links to. Any that have not previously been explored are added to the frontier. This continues until a stopping criterion is fulfilled. For a navigation system, this would be that the desired destination has been retrieved from the frontier. For a web crawler, it may be that a certain number of pages have been indexed or a certain amount of time has elapsed. For a vector database, it may be that is is not possible to get closer to the target.

Different forms of search algorithm are distinguished by the type of data structure they use to represent the frontier. If the frontier is a LIFO stack, we have a *Depth First Search*. In this, the search goes as deep as it can into the network before backtracking and exploring other paths. This can be easily implemented as a recursive algorithm, with the call stack acting as the frontier, and is memory efficient. However, it is probably only suitable for fairly small networks.

If the frontier is a FIFO queue, we have a *Breadth First Search*. This explores all the nodes at a given depth from the starting point before moving on to greater depths. It is suitable for web crawling.

If we can associate a weight with the nodes or edges of the graph, we can use a [priority queue]({% link Key-Algorithms/priority-queues.md %}) for the frontier. This gives us a *Greedy Best First Search*. This can be the most efficient way of finding the best route to a goal, since the most promissing candidates will be explored first, meaning that the target node is likely to be found sooner than by either of the other approaches.

For [QARAC]({% link QARAC/index.md%}), I have written a [web crawler](https://github.com/PeteBleackley/QARAC/blob/main/Crawler.py) based on Greedy Best First Search, to harvest the system's knowledge base. This uses the reliability of the sites that link to a given site as its scoring function. If a given site's inbound links predominately come from unreliable sites, it will be ignored, and the search will terminate when there are no more sites linked from reliable sources to explore.

There are two important variations on Greedy Best First Search that need to be discussed in greater detail. In *Dijkstra's Algorithm*, each edge on the graph is associated with the distance between the two nodes it connects. The weigh of a node on the frontier is the shortest distance so far found to it from the starting node (that is the sum or the distances over the links taken to get to it from the starting node), and will be updated if a shorter path is found. Dijkstra's Algorithm terminates when all nodes have been visited, and returns a *shortest path tree*, representing the best route from the starting point to each of the other nodes in the network.

*A\* Search* is used to find the best route between the starting point and a desired end point. Here the weight used is the sum of two terms. The first is the length for the path from the start node to the candidate node, and the second is an estimate of the distance from the candidate node to the target. This distance is estimated by an *acceptable heuristic*, which can be guaranteed to be equal or less than the true distance. For navigation, the [Euclidean distance]({% link Key-Algorithms/metrics.md %}) is an acceptable heuristic.

by [Dr Peter J Bleackley]({% link index.md %})

[Key Algorithms]({% link Key-Algorithms/index.md %})

Search and Navigation
: [Vector Search Trees]({% link Key-Algorithms/vector-search-trees.md %})
: [Priority Queues]({% link Key-Algorithms/priority-queues.md %})
: *Graph Search Algorithms*
