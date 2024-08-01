---
title: Key Algorithms - Outlier Detection
---

{% include maths.html %}

# Outlier Detection

Many datasets contain *outliers*, datapoints which do not fit the general pattern of the observations. This may be due to errors in the data collection, in which case removing these datapoints will make models fitted to the data more robust and reduce the risk of overfitting. In other cases, the outliers themselves are the signal we want to detect.

One method for doing this is *Isolation Forests*. As the name implies, it is related to the [Random Forest]({% link Key-Algorithms/random-forests.md %}) algorithm. It fits a forest of (usually around 100 ) random decision trees to the dataset by the following method.

1. Pick a feature at random
2. Pick a random threshold in the range of that feature
3. Partition the data at that threshold
4. Repeat the process for each partition

We can then calculate an anomaly score for each datapoint. This is the depth in the decision tree at which a datapoint becomes isolated from the rest of the dataset. The mean of this score over all the trees gives a robust estimator of how easily a datapoint can be separated from the rest. The advantages of this method are that it makes no assumptions about the underlying distribution of the data, and that is explainable, in that the features which are most likely to contribute to a datapoint being isolated can be identified from the decision trees.

I used Isolation Forests in my work at [Amey Strategic Consulting]({% link Portfolio/amey.md %}) to identify faulty traffic flow sensors in the Strategic Road Network.

Another method that makes no assumptions about the underlying distribution is *Local Outlier Factors*. This calculates how different data points are from their local neighbourhood. First we calculated the distances $S_{i,j}$ between datapoints in the sample using some appropriate [metric]({% link Key-Algorithms/metrics.md %}) (this requires all variables to be appropriately scaled) and identify the $N$ nearest neighbours of each datapoint (usually 20). We then calculate the *local density* $D_{i}$ for each datapoint. This is the inverse of the mean of the distances between the point and each of its neighbours.
$$D_{i} = \frac{N}{\sum_{k} S_{i,k}}$$ where $k$ are the indices of the points neighbours. We can then calculate the *Local Outlier Factor* $\mathrm{LOF}$ for each datapoint. This is the mean of the ratio between the datapoints local density and that of each of its neighbours, *ie* 
$$\mathrm{LOF} = \frac{\sum_{k} \frac{D_{i}}{D_{k}}}{N}$$

Samples whose Local Outlier Factor is below a given threshold (*ie* those whose local density is lower than that of their neighbours) can be identified as outliers.

If we can assume that the data are drawn from a multivariate Gaussian distribution, we can use an *Eliptic Envelope* method. For a sample of size $N$ with $d$ dimensions, we chose a sample size $h$ such that 
$$\frac{\N+d+1}{2} < h < N$$
We then select a large number of subsamples of size $h$ from the dataset, and calculate the mean and covariance of each. The one where the covariance has the smallest determinant is the one least likely to contain outliers. Datapoints with a large Mahalanobis distance from the mean of this sample are therefore likely to be outliers.

Of these methods, I'd expect Isolation Forests to be the one most likely to be useful in the widest variety of circumstances.

by [Dr Peter J Bleackley]({% link index.md %})

[Key Algorithms]({% link Key-Algorithms/index.md %})


Missing and Anomalous Data 
: *Outlier Detection*
: [Imputation]({% link Key-Algorithms/imputation.md %})
