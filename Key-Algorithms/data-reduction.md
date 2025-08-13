---
title: Key Algorithms - Data Reduction
---

{% include maths.html %}

# Data Reduction

Datasets that involve a large number of features suffer from *The Curse of Dimensionality*, where, as the number of features increases, it becomes harder and harder to use them to define a meaningful [measure of distance]({% link Key-Algorithms/metrics.md %}) between the samples. It becomes necessary to map the data into a smaller number of dimensions. To do this, we need to find mathematical relationships between the features that can be used to form a more economical representation of the data.

The most common way of doing this is *Principal Component Analysis* (PCA), which captures linear relationships between features. This starts by calculating the covariance of the features

$$\mathbf{\Sigma} = \frac{\sum_{i} (\vec{x_{i}} - \bar{\vec{x}}) \otimes (\vec{x_{i}} - \bar{\vec{x}})}{N}$$

where $\vec{x_{i}}$ is a sample, $\bar{\vec{x}}$ is the mean of the samples, and $N$ is the number of samples. We then calculate the eigenvalues and eigenvectors of this matrix. Each eigenvalue quantifies how much of the variance of the data the associated eigenvector explains. Hopefully, the larger eigenvectors will encode the useful signals in the data and the smaller ones mainly contain noise, which we can filter out. Therefore, if we take the eigenvectors corresponding to the $m$ largest eigenvalues (out of the original $M$ features), we can use them to form an $M \times m$ projection matrix $\mathbf{P}$. We can then project the data into a lower dimension by calculating
$$\vec{x_{i}}^{\prime} = (\vec{x_{i}} - \bar{\vec{x}}) \cdot \mathbf{P}$$

We may chose $m$ by examining a line chart of the eigenvalues in increasing order and looking for an *elbow* where the slope suddenly increases, or by maximising the amount of variance explained while minimising the number of components retained, as described in [How Many Components?]({% link how_many_components.md %}).

A related technique, *Independent component analysis*, seeks to maximise the statistical independence between the projected components rather than the explained variance. This is often used in signal processing.

This works well when the data is dense, and when the classes we want to find in the data are linearly separable. When the data is sparse, we instead use a technique called *Singular Value Decomposition*. Given an $N \times M$ matrix $\mathbf{X}$, we decompose it into an $N \times m$ matrix $\mathbf{U}$, an $m \times m$ matrix $\mathbf{\Sigma}$ and an $M \times m$ matrix $\mathbf{V}$ such that

$$\mathbf{X} \approx \mathbf{U} \cdot \mathbf{\Sigma} \cdot \mathbf{V}^{T}$$

These have the additional properties that $\mathbf{U}$ and $\mathbf{V}$ are *unitary matrices*, that is $$\mathbf{U} \cdot \mathbf{U}^{T} = \mathbf{I}$$ and $$\mathbf{V} \cdot \mathbf{V}^{T} = \mathbf{I}$$
The matrix $\mathbf{\Sigma}$ is zero everywhere except along its leading diagonal. The values along the leading diagonal are known as *singular values*, and act like the eigenvalues in principal component analysis. For a full singular value decomposition, $m=M$ and the product of the matrices is exactly equal to $\mathbf{X}$, but for data reduction we use truncated singular value decomposition, using only the largest $m$ singular values.

$\mathbf{U}$ and $\mathbf{V}$ are the left and right singular vectors, and represent the mapping of the datapoints and the features into the lower-dimensional space respectively.

In the case where the classes are not linearly separable, we need to capture non-linear relationships between the features. The simplest way doing this is *kernel PCA*. This relies on the fact that there is normally a way to project the data into a higher dimensional space so that it becomes linearly separable. To illustrate this, consider a set of concentric circles in a plane. If we add the distance from the centre as a third dimension, the circles appear as separate layers.

But wait. Why are we projecting into a higher-dimensional space when we want to reduce the number of dimensions? Well, we don't actually do this. Instead, we define a *kernel function* $f(\vec{x},\vec{y})$, which corresponds to the distance between two points $\vec{x}$ and $\vec{y}$ in the higher-dimensional space. We then obtain the $N \times N$ matrix

$$\mathbf{F}_{i,j} = f(\vec{x_{i}},\vec{x_{j}})$$

We then obtain the eigenvalues and eigenvectors of this matrix. The eigenvectors corresponding to the $m$ largest eigenvalues form an $N \times m$ matrix whose rows correspond to vectors we would obtain if we carried out PCA in the higher-dimensional space. Unfortunately, for a large dataset, this is more computationally intensive that standard PCA.

There are a number of other techniques for using non-linear relationships in data reduction, collectively known as *manifold learning*, but this article would get a bit too long if we tried to cover them all. However, one that is of particular interest is *t-distributed Stochastic Neighbour Embedding* (t-SNE). This tries to map datapoints to a lower dimension so that the statistical distribution of distances between points in the lower dimension is similar to that in the higher dimension. It is sensitive to the local structure of the data, and so useful for exploratory visualisations.

I used several of these techniques in my work at [Pentland Brands]({% link Portfolio/pentland-brands.md %}). Implementations can be found in Scikit-Learn's [decomposition](https://scikit-learn.org/stable/modules/decomposition.html) and [manifold](https://scikit-learn.org/stable/modules/manifold.html) modules.

By [Dr Peter J Bleackley]({% link index.md %})
 
 [Key Algorithms]({% link Key-Algorithms/index.md %})
 
 Measurements
: [Similarity and Difference Metrics]({% link Key-Algorithms/metrics.md %})
: [TF-IDF]({% link Key-Algorithms/tf-idf.md %})
: *Data Reduction*
: [Latent Semantic Indexing]({%link Key-Algorithms/latent-semantic-indexing.md %})
: [Information Theory]({% link Key-Algorithms/information-theory.md %})

