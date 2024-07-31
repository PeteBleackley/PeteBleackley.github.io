title: Key Algorithms - Expectation Maximisation

# Expectation Maximisation

Consider a dataset where each datapoint $\mathbf{x}$ is associated with an unobserved *latent variable* $z$. We wish to fit a model that accounts for these latent varibles. Obviously, if we knew the values of $z$ we would be able to fit a supervised classifier model to predict them, but since they are unknown values that must be estimated by the model, we must estimate the latent variables and fit then model parameters at the same time.

The method we use to do this is called *Expectation Maximsation*. It iterates over two steps

Expectation step (E-step)
: Estimate the latent varibles given the current model parameters

Maximisation step (M-step)
: Update the model parameters to maximise the likelihood of the data given the current estimates of the latent variables

until the model converges (the improvement in the likelihood is less than a given threshold).

[K-means clustering]({% link Key-Algorithms/k-means.md %}) is a simple form of expectation maximisation, where the latent variables are the cluster labels, the model parameters are the centroids, the expectation step consists of assigning data points to the cluster with the nearest centroid, and the maximisation step consists of recalculating the centroids. It assumes that all clusters have the same variance, and makes a hard assignment of cluster labels, which means that the uncertainty in the assignments is not qunatified.

These assumptions can be relaxed by a *Gaussian Mixture Models*. This assumes that each datapoint is drawn from one of several $k$-dimensional normal distributions 

$$P(\vec{x} \mid z) = \frac{e^{-(\vec{x}-\vec{\mu}_{z}) \cdot \mathbf{\Sigma}^{-1}_{z} \cdot (\vec{x}-\vec{\mu}_{z})}}{\sqrt{(2 \pi)^{k} |\mathbf{\Sigma}_{z}|}}$$

where $\vec{\mu}_{z}$ and $\mathbf{\Sigma}_{z}$ are the mean and covariance of distribution $z$ respectively. The prior probability of the clusters is $P(z)$

In the expectation step, we calculate cluster membership probabilites according to [Bayes' theorem]({% link Key-Algorithms/bayes-theorem.md %})

$$P(z \mid \vec{x}) = \frac{P(z) P(\vec{x} \mid z)}{\sum_{z} P(z) P (\vec{x} \mid z)}$$

We can the use these probabilites in the maximisation step to update the model parameters using weighted means and covariances

$$\vec{\mu}_{z} = \frac{\sum_{i} \vec{x}_{i} P(z \mid \vec{x}_{i})}{\sum_{i} P(z \mid \vec{x}_{i})}$$

$$\mathbf{\Sigma}_{z} = \frac{\sum_{i} (\vec{x}_{i} - \vec{\mu}_z) \otimes (\vec{x}_{i} - \vec{\mu}_{z}) P(z \mid \vec{x}_{i})}{\sum{i} P(z \mid \vec{x}_{i})}$$

$$P(z) = \frac{\sum_{i} P(z \mid \vec{x}_{i})}{N}$$ where $N$ is the number of datapoints.

Gaussian mixture models can be seen as a more rigourous version of K-Means, however, in high dimensions there is a risk of the covariance matrices becoming singular (all the datapoints for a particular cluster lie in a lower-dimensional subspace), so it is a good idea to apply [data reduction]({% link Key-Algorithms/data-reduction.md %}) first.

Mixture models of other distributions are possible, but may require [gradient descent]({% link Key-Algorithms/gradient-descent.md %}) or [Markov Chain Monte Carlo]({% link Key-Algorithms/markov-chain-monte-carlo.md %}) in the maximisation step.

The [SentencePiece]({% link Key-Algorithms/tokenizers.md) tokenizer uses expectation maximisation to calcuate the token frequency distribution. The expectation step calculates the token probabilites given the current maximum likelihood segmentation of the input, while the maximisation step uses the [Viterbi algorithm]({% link Key-Algorithms/viterbi-algorithm.md %}) to calculate a new maximum likelihood segmentation.

by [Dr Peter J Bleackley]({% link index.md %})

[Key Algorithms]({% link Key-Algorithms/index.md %})

Unsupervised Learning
: [Hierarchical Clustering]({% link Key-Algorithms/hierarchical-clustering.md %})
: [K-Means Clustering]({% link Key-Algorithms/k-means.md %})
: *Expectation Maximisation*
