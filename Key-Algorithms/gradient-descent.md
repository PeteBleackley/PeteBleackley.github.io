title: Key Algorithms - Gradient Descent
# Gradient Descent

In several of these articles, we have mentioned the need to train models. For many models, including [Logistic Regression]({% link Key-Algorithms/logistic-regression.md %}), [Linear Regression]({% link Key-Algorithms/linear-regression.md %}), and most forms of neural network, this is done by some form of *Gradient Descent*. Given a model
$$\mathbf{Y} = f(\mathbf{X},\mathbf{W})$$
where $\mathbf{Y}$ are the desired outputs, $\mathbf{X}$ are the inputs, and $\mathbf{W}$ are the model's weights, we first calculate a loss function
$$\mathcal{L}(\mathbf{Y},f(\mathbf{X}, \mathbf{W}))$$
which measures the deviation of the predicted results from the actual values in the training data. We then calculate the gradient of this with respect to the weights.
$$\mathbf{G} = \frac{\partial \mathcal{L}}{\partial \mathbf{W}}$$
This is generally calculated using the [Chain Rule]({% link Key-Algorithmschain-rule.md %}). We then update the weights as 
$$\mathbf{W} \rightarrow \mathbf{W} - \eta \mathbf{G}$$
where $\eta$ is a small constant known as the *learning rate*. This process is then iterated over a number of *epochs*, or until the loss has converged to a minimum and no further improvement can be found.

Gradient descent methods are analogous to a physical system in which the loss function represents the potential energy of a particle whose position is given by the model's parameters.

Calculating the loss and gradient over the whole dataset as described (*Batch Gradient Descent*) above is only really tractable for simple models and small datasets. For larger datasets and more complex datasets, memory requirements would be prohibitive, so it is easier to iterate over the dataset at each epoch, calculating the update for each sample individually. However, since the gradient is in general a non-linear function of the input, wieghts and outputs, the update calculated for each datapoint will depend on the updates performed on previous datapoints. If we were to iterate over the dataset in a fixed order, this would lead to a risk of creating systematic errors in the fit, which could lead to the model converging to a local minimum, where it is not optimal, but cannot be further improved by gradient descent.

To avoid this, *Stochastic Gradient Descent* shuffles the dataset between each epoch. As the samples are now visited in a different random order each time, any systematic errors that may arise from the order of iteration are smoothed out.

However, since Stochastic Gradient Descent involves calculating an update for each datapoint in the sample, it is more computationally expensive than simple gradient descent, and while shuffling the data mitigates the instability caused by iterating over individual data points, it does not eliminate it completely. These problems can be addressed by *Mini-Batch Gradient Descent*, where, after shuffling, the datapoints are grouped into batches, and the update calculated for each batch. Small batches such as 32 samples are found to give a good trade-off between stability and memory use.

The learning rate is an important parameter in gradient descent algorithms. Too low and the algorithms will take a long time to converge, too high and they will tend to overshoot the minimum. [Transfer Learning]({% link Key-Algorithms/transfer-learning.md %}) uses small learning rates to avoid catastrophic forgetting. There are various methods of adapting the leaarning rate as training progresses. The simplest is to use a *Learning Rate Scheduler*, which starts with a higher learning rate to ensure the parameter space is adequately explored, and gradually adjusts it downwards as training progresses. other methods, such as [AdaGrad and RMSProp](https://optimization.cbe.cornell.edu/index.php?title=AdaGrad), which use independent learning rates for each parameter, which are continuously updated based on the gradients previously encountered.

A further adaptation to gradient descent algorithms is to introduce the concept of *momentum*. Whereas in the physical analogy, the methods previously described treat the gradient as the velocity of the particle, momentum based methods such as [Adam](https://optimization.cbe.cornell.edu/index.php?title=Adam) treat it as an acceleration. Given a momentum $\mathbf{M}$ which is the same shape as $\mathbf{W}$, and two learning rates $\alpha$ and $\beta$, ($0 < \beta < 1$) the weights and momentum are updated at each timestep by
$$\mathbf{W} \rightarrow \mathbf{W} - \alpha \mathbf{M}$$
$$\mathbf{M} \rightarrow \beta \mathbf{M} + (1 - \beta) \frac{\partial \mathcal{L}}{\partial \mathbf{W}}$$

By retaining information about previous gradients between timesteps, these models are able to converge more smoothly and quickly. $\beta$ damps the momentum to prevent divergence.

By [Dr Peter J Bleackley]({% link index.md %})
 
 [Key Algorithms]({% link Key-Algorithms/index.md %})

Components of Neural Networks
: [The Chain Rule and Backpropogation]({% link Key-Algorithms/chain-rule.md %})
: [Activation Functions]({% link Key-Algorithms/activation-functions %})
: [Loss Functions]({% link Key-Algorithms/loss-functions.md %})
: *Gradient Descent*
: [Transfer Learning]({% link Key-Algorithms/transfer-learning.md %})
: [Tokenizers]({% Key-Algorithms/link tokenizers.md})
