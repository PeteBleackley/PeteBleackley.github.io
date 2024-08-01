---
title: Key Algorithms - Vector Search Trees
---

{% include maths.html %}

# Vector Search Trees

There are many applications where we need to search a dataset for the nearest neghbours of a given point. For a large dataset, comparing the data point to the entire dataset will be too slow, especially if we need to do it frequently. If we store the dataset to be searched in a tree structure, we can improve the efficiency of queries from $\mathcal{O} (N)$ to $\mathcal{O} (\log N)$.

A simple method to construct the search tree is *KD Trees*. This method iterates over the dimension of the dataset, partitioning it into hyperrectanular blocks. Each of these blocks is partitioned at the median of the datapoints contained in it along the dimension under consideration. Using the median ensures that the number of points in each partition will be balanced. This allows for rapid construction of the search tree, and and rapid searching if the dimensionality of the data is low, but its performance degrades when the number of dimensions in the dataset is large. The documentation for the [SciPy implementation of KD Tree](https://docs.scipy.org/doc/scipy/reference/generated/scipy.spatial.KDTree.html#scipy.spatial.KDTree) notes that *20 is already too large*. Adding new data to the tree after initial construction also runs a high risk of the tree becoming unbalanced.

An alternative, that improves performance at higher dimensionalities, is *Ball Trees*. In this, each node represents a ball of centroid $\vec{C}$ and radius $r$. Data is assigned to the nodes in such a way as to minimise the hypervolume of the balls. Several methods for doing this are available, as detailed by Stephen M. Omohundro in [Five Balltree Construction Algorithms](https://ftp.icsi.berkeley.edu/ftp/pub/techreports/1989/tr-89-063.pdf). The one used in the [Scikit-Learn implementation of Ball Trees](https://scikit-learn.org/stable/modules/generated/sklearn.neighbors.BallTree.html#sklearn.neighbors.BallTree) is a variation on the KD Tree construction algorithm, where instead of iterating through the dimensions in a fixed order, each node is partitioned along the dimension in which the spread of its datapoints is greatest. Another method is an *online insertion algorithm*, which is suitable for when we want to continually add new data to the search tree. Given a tree, each new node is added to the tree in the position that minimises the increase in volume of the nodes that contain it. It is also possible to build a Ball Tree bottom up, with a method based on [Hierachical Clustering]({% link Key-Algorithms/hierarchical-clustering.md %}).

Another method for constructing search trees is *ANNOY* (Approximate Nearest Neighbours Oh Yeah), which was developed by Erik Bernhardsson at Spotify, who needed to be able to search large datasets of high dimensional datasets as quickly as possible for music recommendations. In this method, the dataset is recursively partitioned by picking two datapoints at random from each existing partition and splitting the partition midway between them. The random assignment of the partitions means that it is possible for the nearest neighbour of a point to fall into a different partition. Therefore, an ensemble of trees, similar to a [Random Forest]({% link Key-Algorithms/random-forests.md %}) is constructed. We can then find a candidate nearest neighbour from each tree and select the best. The randomness of the algorithm makes the matches approximate, rather than exact, but for many applications this doesn't matter
Here is [Eric Berharsson's own description of ANNOY](https://erikbern.com/2015/10/01/nearest-neighbors-and-vector-models-part-2-how-to-search-in-high-dimensional-spaces.html). There's a [Python implementation of ANNOY](https://pypi.org/project/annoy/1.0.3/) on PyPI and it can be used to search word vectors or document vectors in [Gensim](https://radimrehurek.com/gensim/similarities/annoy.html)

by [Dr Peter J Bleackley]({% link index.md %})

[Key Algorithms]({% link Key-Algorithms/index.md %})

Search and Navigation
: *Vector Search Trees*
: [Priority Queues]({% link Key-Algorithms/priority-queues.md %})
: [Graph Search Algorithms]({% link Key-Algorithms/graph-search-algorithms.md %})
