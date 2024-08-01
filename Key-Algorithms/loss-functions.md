---
title: Key Algorithms - Loss Functions
---
{% include maths.html %}

# Loss Functions

In several articles, we have mentioned *Loss Functions*. These are the functions we aim to minimise when fitting the model. These are functions $$\mathcal{L}(y_{t},y_{p})$$ where $y_{t}$ is the true value of the target variable and $y_{p}$ is the predicted output. They should have the following properties.

1. They should have a global minimum when $y_{t} = y_{p}$ and be strictly increasing elsewhere.
2. They should be differentiable, so that we can apply [backpropogation]({% link Key-Algorithms/chain-rule.md %}) and [gradient descent]({% link Key-Algorithms/gradient-descent.md %})

They similar to, and to some extent, overlap with [Similarity and Distance Metrics]({% link Key-Algorithms/metrics.md %}).

For regression problems, an obvious choice of loss function is the *Mean Squared Error*

$$\mathcal{L}(y_{t},y_{p}) = (y_{t} - y_{p})^{2}$$

This is easily differentiable

$$\frac{d \mathcal{L}}{d y_{p}} = 2(y_{p} - y_{t})$$

[Linear Regression]({% link Key-Algorithms/linear-regression.md %}) models usually use some variation of this, often with a regularisation penalty to prevent overfitting. However, since the loss is quadratic in the deviation of the prediction from the true value, this function is sensitive to outliers. The *Mean Absolute Error*
$$\mathcal{L}(y_{t},y_{p}) = |y_{t} - y_{p}|$$ would avoid this problem, but is not differentialble at $y_{t} = y_{p}$. This problem can be addressed by using the *Huber Loss*

$$\mathcal{L}(y_{t},y_{p}) = \min(\frac{(y_{t} - y_{p})^2)}{2}, \delta (|y_{t} - y_{p}| - \frac{\delta}{2}))$$

This behaves like Mean Squared Error for $\mid y_{t} - y_{p} \mid < \delta$, and like Mean Absolute Error for $ \mid y_{t} - y_{p} \mid > \delta$, where $\delta$ is a scale above which we wish to reduce the influence of outliers. The derivative is given by

$$\frac{d \mathcal{L}}{d y_{p}} = \max(-\delta,\min(\delta,\delta (y_{p} - y_{t})))$$

These loss functions assume that the errors are normally distributed with constant variance. If we expect a different distribution, we can use a *Negative Log Likelihood Loss* based on the expected distribution. For example, the if predicted values are expected to be drawn from the Poisson Distribution
$$P(k \mid \lambda) = \frac{\lambda^{k} e^{-\lambda}}{k!}$$

we can, by setting $\lambda = y_{p}$ and $k = y_{t}$ use the *Poisson Loss*

$$\mathcal{L}(y_{t},y_{p}) = -ln(P(y_{t} \mid y_{p}) = \ln(y_{t}!) + y_{p} - y_{t} \ln y_{p}$$

The derivative of this is 

$$\frac{d \mathcal{L}}{d y_{p}} = 1 - y_{t}/y_{p}$$

If the variance of the predictions is not constant, but is itself predicted by the model, we may use the *Gaussian Negative Log Likelihood Loss*

$$\mathcal{L}(y_{t},y_{p},\sigma) = \frac{1}{2}\left( 2 \ln(\sigma) +\left(\frac{y_{t} - y_{p}}{\sigma} \right)^{2} \right)$$
where $\sigma^{2}$ is the variance

The derivatives are
$$\frac{\partial \mathcal{L}}{\partial y_{p}} = \frac{y_{p} - y_{t}}{\sigma^{2}}$$
$$\frac{\partial \mathcal{L}}{\partial \sigma} = \frac{1}{\sigma}\left(1 - \left(\frac{y_{t} - y_{p}}{\sigma} \right)^{2} \right)$$

For classification problems, we usually use a *Cross Entropy Loss*. This assumes that the prediction is a probability distribution over the possible classes, and that the true value is represented by either a binary flag or a one-hot encoded value. In the case of binary classification, we use the *Binary Cross Entropy Loss*
$$\mathcal{L}(y_{t},y_{p}) = y_{t} \ln(y_{p}) + (1 - y_{t}) \ln(1 - y_{p})$$
for which the derivative is
$$\frac{d \mathcal{L}}{d y_{p}} = \frac{y_{t}}{y_{p}} - \frac{1-y_{t}}{1-y_{p}}$$

Where there are more than two classes, we use the *Categorical Cross Entropy Loss*. For this, $\vec{y}_{p}$ represents a probability distribution over classes $i$ such that
$$y_{pi} = P(i)$$
$\vec{y}_{t}$ is a one hot encoded representeation of the correct class $j$, such that
$$y_{ti} = \delta_{ij}$$ where $\delta_{ij}$ is the Kroneker delta function

We then have
$$\mathcal{L}(y_{t},y_{p}) = -\ln(\vec{y}_{t} \cdot \vec{y}_{p}) \\
= -\ln(\sum_{i} \delta_{ij} P(i) \\
= - \ln(P(j))$$

and 

$$\frac{d \mathcal{L}}{d \vec{y}_{p}} = -\frac{1}{P(j)}$$

Two variations of this are worth noting. In *Sparse Categorical Cross Entropy*, the true values are given as integer indexes rather than one-hot encoded. This is useful when there are a large number of categories, such as in Natural Language Processing. Secondly, since the predictions are usually obtained from a [softmax function]({% link Key-Algorithms/logistic-regression.md %}), it is possible to supply them as logits rather than probabilities.

If the values of $y_{t}$ are themselves probabilities, an appropriate loss function is the *Kullback-Liebler Divergence Loss*
$$\mathcal{L}(y_{t},y_{p}) = y_{t}(\ln(y_{t} - \ln(y_{p})$$

The derivative is
$$\frac{d \mathcal{L}}{d y_{p}} = -\frac{y_{t}}{y_{p}}$$

Cross-entropy and Kullback-Liebler divergence are both derived from [Information Theory]({% link Key-Algorithms/information-theory.md %})

Both [PyTorch](https://pytorch.org/docs/stable/nn.html#loss-functions) and [Keras](https://keras.io/api/losses/) provide a wide variety of loss functions.

While the serve similar purposes, it is important not to conflate loss functions with evaluation metrics. Just as the data used to test the performance of the model should be independent of that used to train it, so should the metrics, as far as possible, to ensure a rigorous evaluation.

By [Dr Peter J Bleackley]({% link index.md %})
 
[Key Algorithms]({% link Key-Algorithms/index.md %})


Components of Neural Networks
: [The Chain Rule and Backpropogation]({% link Key-Algorithms/chain-rule.md %})
: [Activation Functions]({% link Key-Algorithms/activation-functions.md %})
: *Loss Functions*
: [Gradient Descent]({% link Key-Algorithms/gradient-descent.md %})
: [Transfer Learning]({% link Key-Algorithms/transfer-learning.md %})
: [Tokenizers]({% link Key-Algorithms/tokenizers.md %})
