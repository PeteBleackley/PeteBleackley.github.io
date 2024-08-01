---
title: Key Algorithms - Hierarchical Clustering
---
{% include maths.html %}

# Hierarchical Clustering

When exploring a dataset, it is often useful to identify what groups or *clusters* of items may exist within the data. This is know as *unsupervised* learning, since it attempts to learn what classes exist within the data without prior knowledge of what they are, as opposed to *supervised learning* (classification), which trains a model to identify known classes in the dataset.

A simple method for this is *Hierarchical Clustering*. This arranges the datapoints in a tree structure by the following method.

1. Assign each data point to a *leaf node*
2. Calculate the distances between the nodes using an appropriate [metric]({% link Key-Algorithms/metrics.md %})
3. Create the *parent node* of the two nodes that are closest to each other, and replace its two *daughter nodes* with it.
4. Calculate the distances of the new node to each of the remaining nodes in the dataset
5. Repeat from step 3 until all all the nodes have been merged into a single tree.

At stage 4, there are a number of different *linkage methods* for calculating the new distances. The main ones are

Single linkage
: use the minimum distance between two points in each cluster

Complete (or maximum) linkage
: use the maximum distance between two points in each cluster

Average linkage
: use the average distance between points in the two clusters. With Euclidean distances, this can be simplified to the distance between the centroids of the clusters

Ward linkage
: calculate distances between clusters recursively with the formula
$$d(u,v) = \sqrt{ \frac{\left(n_{v} + n_{s}\right) d(s,v)^{2} + \left(n_{v} + n_{t}\right) d(t,v)^{2} - n_{v} d(s,t)^{2}}{n_{s} + n_{t} + n_{v}}}$$
where $u$ is the cluster formed by merging $s$ and $t$, $v$ is another cluster, and $n_{c}$ is the number of datapoints in cluster $c$. The distance between two leaf nodes is euclidean. This has the property of minimising the variance of the new cluster.

Ward linkage is the technique most likely to give even cluster sizes, while single linkage is the one most useful when cluster shapes are likely to be irregular.

One problem with Hierarchical Clustering is that, as described above, it does not produce discrete clusters. One way to address this is to visualise the data and chose a number of clusters, and then terminate the clustering early when that number of clusters is reached. The number of clusters may be chosen by visualising the data, either by using [t-SNE]({% link Key-Algorithms/data-reduction.md %}) or by performing an initial clustering and plotting the tree structure on a dendrogram. Another method is to chose a distance threshold, and not merge clusters further apart than this. It would be necessary to know the statistical distribution of distances between clusters to choose an appropriate threshold. While I have not seen this implemented, it would be theoretically possible to use the [Bayesian Information Criterion]({$ link Key-Algorithms/information-theory.md %}) to decide when to separate clusters - this approach would be most useful when Ward linkage was used.

In [Clustering Proteins in Breast Cancer Patients]({% link Data-Science-Notebooks/clustering-proteins.md %}) I used Hierarchical Clustering to identify groups of proteins whose activity was related in patients.

Implementations of Heirarchical Clustering can be found in [Scipy](https://docs.scipy.org/doc/scipy/reference/cluster.hierarchy.html) and [Scikit-Learn](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.AgglomerativeClustering.html)

by [Dr Peter J Bleackley]({% link index.md %})

[Key Algorithms]({% link Key-Algorithms/index.md %})

Unsupervised Learning
: *Hierarchical Clustering*
: [K-Means Clustering]({% link Key-Algorithms/k-means.md %})
: [Expectation Maximisation]({% link Key-Algorithms/expectation-maximisation.md %})

