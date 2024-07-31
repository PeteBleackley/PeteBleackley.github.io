title: Key Algorithms - Logistic Regression

#Logistic Regression

We have previously discussed classification in relation to [Bayes' Theorem]({% link bayes-theorem.md %}). Logistic regression provides a different method for relating probabilities to observations.

We start with the *logistic function*
$$p = \frac{1}{1+e^{-q}}$$, where $q$ is a quantity we call a *logit*. This has the property that as $q \rightarrow \infty$, $p \rightarrow 1$ and as $q \rightarrow -\infty$, $p \rightarrow 0$, so it can be used to model a probability. If we wish to calculate the probabilities of more than one class, we can generalise this with the *softmax function*
$$p_{i} = \frac{e^{q_{i}}}{\sum_{i} e^{q_{i}}}$$ where $p_{i}$ and  $q_{i}$ represent the probabilities and logits for each class $i$ respectively.

But what are the logits? In the basic implementation of logistic regression, they are a linear function of some observations. Given a vector $\vec{x}$ of observations, we may model the logits as $$q = \vec{w} \cdot \vec{x} + b$$ for the binary case and $$\vec{q} = \mathbf{W} \cdot \vec{x} + \vec {b}$$ in the multiclass case. where $\vec{w}$ and $\mathbf{W}$ are *weights* and $b$ and $\vec{b}$ are biases. In terms of Bayes' Theorem.
$$\vec{b} = \ln P(H)$$ and $$\mathbf{W} \cdot \vec{x} = \ln P(\vec{x} \mid H)$$

We fit the weights and biases by minimising the *cross-entropy loss*
$$L = -\sum_{j} \ln p_{j,c}$$ where $c$ is the correct class for the example $j$ in the training dataset. 

This works well as a simple classifier under two conditions

1. The classes are fairly evenly balanced
2. The classes are linearly seperable

If there is a strong imbalance between the classes, the bias will tend to dominate over the weights, and the rarer classes will never be predicted. To mitigate this, is is possible to undersample the more common classes or oversample the rarer ones before training.

If the classes are not linearly seperable, it's necessary to transform the data into a space where they are. This may be done by applying $$\vec{x^{\prime}} = f(\mathbf{M} \cdot \vec{x})$$ where $f$ is some non-linear function and $\mathbf{M}$ is a matrix of weights. We may in fact apply several layers of similar transformations, each with its own set of weight parameters. That is the basis of [neural networks]({% link Key-Algorithms/multi-leyer-perceptron.md %}).

by [Dr Peter J Bleackley]({% link index.md %})

[Key Algorithms]({% link Key-Algorithms/index.md %})

Simple Supervised Learning Models
: *Logistic Regression*
: [Linear Regression]({% link Key-Algorithms/linear-regression.md})
: [Random Forests]({% link Key-Algorithms/random-forests.md})
