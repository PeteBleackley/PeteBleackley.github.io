title: Key Algorithms - The Chain Rule and Backpropogation

# The Chain Rule and Backpropogation

In the article about [Logistic Regression]({% link Key-Algorithms/logistic-regression.md %}), we mentioned that logistic regression and neural networks are fit by minimising a [loss function]({% link Key-Algorithms/loss-functions.md %}). In order to do this, we need to calculate the gradient of the loss function with respect to the parameters. This tells us how we can adjust the parameters to reduce the loss. 

Since the functions we want to optimise in machine learning problems can usually be expressed as a function of a function. To differentiate this, we use the *chain rule*
$$\frac{df(g(x))}{dx} = \frac{df}{dg}\frac{dg}{dx}$$

To illustrate this, let's see how we can use it to differentiate the cross-entropy loss
$$L = -\ln p_{c}$$ with respect to the weights $\mathbf{W}$ of a logistic regression model. First we differentiate the loss with respect to the probability of the correct class.
$$\frac{dL}{dp_{c}} = -\frac{1}{p_{c}}$$

Then we need to differentiate the probability with respect to each of the logits $q_{i}$
$$p_{c} = \frac{e^{q_{c}}}{\sum_{i} e^{q_{i}}} \\
\frac{\partial p_{c}}{\partial q_{i}} = \frac{\delta_{ic} e^{q_{c}} \sum_{i} e^{q_{i}} - e^{q_{c}} e^{q_{i}}}{\left( \sum_{i} e^{q_{i}} \right)^{2}} \\
= \frac{e^{q_{c}}}{\sum_{i} e^{q_{i}}} \frac{\delta_{ic} \sum_{i} e^{q_{i}} - e^{q_{i}}}{\sum_{i} e^{q_{i}}} \\
= p_{c}(\delta_{ic} - p_{i}) $$
where $\delta_{ic}$ is the *Kroneker delta function*, which is 1 if $i=c$ and 0 otherwise.
(As an aside, functions whose derivative can be expressed in terms of their output are commonly used in machine learning, because they make differentiation easier. Such functions are often derived from the exponential function in some way).

Then, we need to differentiate the logits with respect to the weights
$$\vec{q} = \mathbf{W} \cdot \vec{x} + \vec{b} \\
\frac{d \vec{q}}{d \mathbf{W}} = \vec{x}$$

Finally, we can combine these derivatives using the chain rule
$$\frac{dL}{d\mathbf{W}} = \frac{dL}{dp_{c}}\frac{dp_{c}}{d\vec{q}}\frac{d\vec{q}}{d\mathbf{W}} \\
=-\frac{1}{p_{c}}p_{c}(\delta_{ic}-\vec{p}) \otimes \vec{x} \\
=(\vec{p} - \delta_{ic}) \otimes \vec{x}$$ where $\otimes$ denotes the outer product.

For a deeper neural network, we use the fact that each layer $n$ of the network can be treated as a function $$\vec{x}_{n+1} = f_{n}(\mathbf{W}_{n} \cdot \vec{x}_{n} + \vec{b}_{n})$$ and apply the chain rule recursively to calculate the gradient of the loss with respect to each layer's weights and biases. This recursive application of the chain rule is known as *backpropogation*, and is the basis of most neural network optimisation algorithms.

Of course, very few data scientists ever need to do this themselves on a day-to-day basis, because automatic differentiation and backpropogation are provided by machine learning software libraries, but it's still useful to understand how it works.

By [Dr Peter J Bleackley]({% link index.md %})
 
 [Key Algorithms]({% link Key-Algorithms/index.md %})

Components of Neural Networks
: *The Chain Rule and Backpropogation*
: [Activation Functions]({% link Key-Algorithms/activation-functions.md %})
: [Loss Functions]({% link Key-Algorithms/loss-functions.md %})
: [Gradient Descent]({% link Key-Algorithms/gradient-descent.md %})
: [Transfer Learning]({% link Key-Algorithms/transfer-learning.md %})
: [Tokenizers]({% link Key-Algorithms/tokenizers.md %})
