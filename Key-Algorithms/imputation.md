title: Key Algorithms - Imputation

# Imputation

Real world data is often messy. Values may often be missing, or erroneous, which may be flagged by [Outlier Detection]({% link Key-Algorithms/outlier-detection.md %}). If only a small number of values are missing, it may be possible to simply omit them from the dataset, but if missing values are frequent, this might lead to us losing too much of the dataset. We need a way to estimate the missing values. Such methods are known as *Imputation*.

Imputation methods can be divided into *Single Imputation*, which estimate missing values for a single variable, and *Multiple Imputation*, which estimate values for several missing values. 
Simgle Imputation methods usually substitute all the missing values with a single value, which may be the mean, median or mode of the varible's known values. This runs the risk of introducing bias into the dataset, although the median and mode are less likely to intoduce bias than the mean. An alternative is *Random Imputation*, where the missing values are filled with random samples from the variable's probability distribution. This reduces the potential for bias but introduces noise.

*Multiple Imputation* methods infer the unknown values from known values of other variables. One method is to fit a regression model (such as [Linear Regression]({% link Key-Algorithmslinear-regression.md %}) or a [Random Forest]({% link Key-Algorithms random-forests.md %}) regression model) to the known values of the variable in terms of the other variables and interpolate the missing values. One version of this, *MICE* (Multivariate Inputation by Chained Equations), starts by using Single Imputation to provide initial estimates for the missing values. Then for each variable in turn, the imputed values are updated by a regression model. Over a number of iterations, the imputed values are expected to converge to realistic estimates, as each update of one variable improves the regression models used to estimate the others. A fuller description can be found in [Multiple imputation by chained equations: what is it and how does it work?](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3074241/ %})

Another method to predict the missing values is *K Nearest Neighbours*. In this we use an approproiate [metric]({% link Key-Algorithms/metrics.md %}) to find the $K$ most similar datapoints with known values for the missing variable, and use the mean of their values for that variable as the estimate. This avoids the overhead of fitting regression models, but may overfit.

If all the values in the dataset are positive or zero, *Non-Negative Matrix Factorisation* can be used. If the dataset is represented by an $N \times M$ matrix $\mathbf{X}$, we decompose it into an $N \times m$ matrix $\mathbf{U}$  and an $M \times m$ matrix $\mathbf{V}$ such that

$$\mathbf{X} \approx \mathbf{U} \cdot \mathbf{V}^{T}$$

(note the similarity to [Singular Value Decomposition]({% link Key-Algorithms/data-reduction.md) %}). When fitting $\mathbf{U}$ and $\mathbf{V}$ we simply ignore missing values. However, when we reconstruct $\mathbf{X}$ from the factor matrices, it will contain predictions for the missing values, based on correlations found in the dataset. This technique can also be used as a recommendation algorithm, where $\mathbf{X}$ represents users' ratings for various items, and the missing values are ratings we wish to predict.

[Scikit-learn's `impute` module](https://scikit-learn.org/stable/modules/impute.html) contains implementations for some of these techniques.

by [Dr Peter J Bleackley]({% link index.md %})

[Key Algorithms]({% link Key-Algorithms/index.md %})


Missing and Anomalous Data 
: [Outlier Detection]({% link Key-Algorithms/outlier-detection.md %})
: *Imputation*
