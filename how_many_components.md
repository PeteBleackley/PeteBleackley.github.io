---
layout: default
title: How many components?
---

# How Many Components


A common problem in data science is *the curse of dimensionality*. Essentially, the more different variables a dataset encompasses, the more mathematically intractible it is to make measurements based on them all. The usual method for dealing with this problem is [Principal Component Analysis]({% link Key-Algorithms/data-reduction.md %}), which seeks to reduct the data to a lower number of dimension while retaining as much information as possible. The most common method of doing this is as follows.

1. Obtain either the covariance matrix of the variables of a similarity matrix of the observations, using a metric such as cosine similarity
2. Calculate the eigenvalues and eigenvectors of this matrix
3. Use the eigenvectors corresponding to the N largest eigenvalues to form an orthonormal basis

This however, raises the question of how to select an appropriate value of N. Since our aim is to explain the maximum amount of variance with the minimum number of components, a simple approach is to find a maximum in the sum of the proportion of components discarded with the proportion of variance retained, as measured by the eigenvalues. `numpy` helps us with this by returning eigenvalues and eigenvectors in increasing order of eigenvector. The following code illustrates the method

    import numpy
    import numpy.linalg
    
    def reduce_data(X,metric=None):
       """Reduces X to the number of dimensions that retains the maximum amount of infomation for the minimum number of components
       Parameters
       ----------
       X : numpy.ndarray
           (n * m) array containing n rows of m-dimensional observations
       metric : function (optional, default = None)
           Similarity metric. Takes an (n * m) array and returns an (n * n) array of similarities
       Returns
       -------
       numpy.ndarray
           The data reduced to the optimum number of dimensions
           """
       similarity = numpy.cov(X, rowvar=False) if metric is None else metric(X)
       (eigenvalues, eigenvectors) = numpy.linalg.eigh(similarity)
       n = eigenvalues.shape[0]
       excluded = numpy.arange(n)/n
       explained = 1.0 - (eigenvalues.cumsum()/eigenvalues.sum())
       cutoff = (excluded + explained).argmax()
       basis = eigenvectors[:,cutoff:]
       return X.dot(basis) if metric is None else basis
       
by [Dr Peter J Bleackley]({% link index.md %})

