title: Key Algorithms - K-Means Clustering

# K-Means Clustering

In the previous article, we discussed [Hierarchical Clustering]({% link Key-Algorithms/hierarchical-clustering.md %}). Another method commonly used method is the *K-Means* algorithm, which attempts to find $K$ clusters such that this variance within the clusters is minimised. It does this by the following method

1. Given an appropriately scaled dataset, choose $K$ points in the range of the data
2. Assign each point in the dataset to a cluster associated with the nearest of these points, accorting to the [euclidean distance]({% link key-Algorithms/metrics.md %})
3. Recalcuate the points as the means of the datapoints assigned to their clusters
4. Repeat from step 2 until the assignments converge

Several different methods may be used to assign the initial centroids. The *Random Partition* method initially assigns each datapoint to a random cluster and takes the means of those clusters as the starting points. This tends to produce initial centroids close to the centre of the dataset. *Fogy's method* choses $K$ datapoints randomly from the dataset as the initial centroids. This tends to give more widely spaced centroids. A variation of this, the *kmeans++* method, weights the probability of chosing each datapoint as a centroid by the minimum squared distance of that point from the centroids already chosen. This is the default in [Scikit-Learn's implementation of K-means](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.KMeans.html), since it is considered more robust. K-means cannot be guaranteed to converge to the optimal solution and is senitive to its initial conditions, it is common practice to rerun the clustering several times with several different sets of initial centroids, and chose the solution with the lowest variance.

Another issue with K-means is how many clusters to chose. This may be done by visialising the data in advance, or with the *silhouette score*. This is a measure of how much closer a datapoint is to other datapoints in its own cluster than it is to datapoints in other clusters. For a datapoint $i$ which is a member of cluster $C_{k}$, which has $N_{k}$ datapoints assigned to it, we first calculate the mean distance of $i$ from the other members of $C_{k}$

$$a_{i} = \frac{\sum_{j \in C_{k},j \neq i} d(i,j)}{N_{k}-1}$$

where $d(i,j)$ is the distance betwen datapoints $i$ and $j$

We then find the mean distance betwen $i$ and the datapoints in the closest cluster to it other than the one to which it is assigned.

$$b_{i} = \min_{l \neq k} \frac{\sum_{j \in C_{l}} d(i,j)}{N_{l}}$$

The silhouette score for an individual point is then calculated as $$s_{i} = \frac{b_{i} - a_{i}}{\max(b_{i},a_{i})}$$

This has a range of -1 to 1, where a high value would indicate that a datapoint is central to its cluster and a low value that it is peripheral. We may then calculate the mean of $s_{i}$ over the dataset. The optimum number of clusters is the one that maximises this score.

*X-means* is a variant of K-means that aims to select the optimum number of clusters automatically. This follows the following procedure.
1. Perform K-means on the dataset with $K=2$.
2. For each cluster, perform K-means again with $K=2$ for the members of that cluster.
3. Use the [Bayesian Information Criterion]({% link Key-Algorithms/information-theory.md %}) to determine whether this improves the model. Keep subdividing clusters until it does not.
4. When no further subdivision are necessary, use the centroids of the clusters thus obtained as the starting point for a final round of K-means clustering on the full dataset.

K-means clustering requires the clusters to be linearly seperable. If this is not the case, it is necessary to perform [Kernel PCA]({% link Key-Algorithms/data-reduction.md %}) to map the dataset into a space where they are.

by [Dr Peter J Bleackley]({% link index.md %})

[Key Algorithms]({% link Key-Algorithms/index.md %})

Unsupervised Learning
: [Hierarchical Clustering]({% link Key-Algorithms/hierarchical-clustering.md %})
: *K-Means Clustering*
: [Expectation Maximisation]({% link Key-Algorithms/expectation-maximisation.md %})


