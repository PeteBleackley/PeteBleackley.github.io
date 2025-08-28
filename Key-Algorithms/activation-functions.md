---
title: Key Algorithms - Activation Functions
---
{% include maths.html %}
# Activation Functions


In the previous article, discussing [Multi Layer Perceptron]({% link Key-Algorithms/multi-layer-perceptron.md %}) networks, we mentioned activation functions, which allow neural networks to learn non-linear functions. In this article, we'll look at these functions in more detail.

As the name implies, Multi Layer Perceptron networks are based on the *perceptron* model, which fits a function 

$$y = \mathrm{sgn}( \vec{w} \cdot \vec{x} + b)$$

to the data. The activation function for this model is the *Heaviside Step Function*, but since it is not differentiable, we cannot apply the [chain rule]({% link Key-Algorithms/chain-rule.md %}) to it. Therefore, early neural networks used activation function that replaced the sudden step with a smooth transition between positive and negative values. One such function is the *softsign function*

$$f(x) = \frac{x}{|x| + 1}$$

Whose derivative is given by 

$$\frac{df}{dx} = \frac{1}{(|x|+1)^{2}}$$

but more commonly used is the *hyperbolic tangent*

$$\tanh{x} = \frac{e^{x} - e^{-x}}{e^{x} + e^{-x}}$$

for which

$$\frac{df}{dx} = 1-f(x)^{2}$$

This is related to the [logistic function]({% link Key-Algorithms/logistic-regression.md %}) or *sigmoid*

$$\sigma(x) = \frac{1}{1+e^{-x}}$$ by the relationship
$$\tanh{x/2} = 2 \sigma(x) -1$$

Both these functions are 0 at $x=0$ and have the property
$$\lim_{x\to\pm\infty} y = \pm 1$$
and so are known as *saturating functions* as a result. One disadvantage of saturating functions is the *vanishing gradient problem*. Since the gradient of a saturating function tends to zero for large positive or negative $x$, these functions will propagate little information to their inputs during training if they are close to saturation. This makes them unsuitable for use in deep networks. As a result, a variety of *non-saturating functions* are now used.

One of the most common of these is the *Rectified Linear Unit* (ReLU). 
$$f(x) = \max(0,x)$$
The output vector space for a layer with ReLU activation is a piecewise linear function. It has the advantage of computational simplicity, but can suffer from *dead neurons*. Since it can produce arbirtrarity large outputs, but the gradient is zero when the input is less than zero, a neuron with large negative weights can get locked into producing zero outputs. To address this problem a number of variations of ReLu are used, which produce non-zero outputs for negative inputs. *Leaky ReLU* is a piecewise linear function
$$f(x) = \max(\alpha x, x)$$
where $\alpha$ is a constant in the range $0 < \alpha <1$. If we treat $\alpha$ as a trainable parameter, we get *Parametric ReLU* (PReLU).

*Exponential Linear Unit* (ELU) uses an offset exponential function for negative inputs.
$$f(x) = \max(x,e^{x}-1)$$. This means that the activation will saturate for large negative values but not for positive values.

The exponential function 
$$y = e^{x}$$ may also be used as an activation function, but has the disadvantage that its gradients can be arbitrarily large, so this can lead to an *exploding gradient problem*, where gradients increase without limit during training, leading to instability and the risk of numerical overflows. A more stable alternative is the *softsum function*
$$f(x) = \ln(e^{x}+1)$$
This can be seen as a fully continuous alternative to ReLU. The gradient is given by
$$\frac{df}{dx} = \frac{e^{x}}{e^{x}+1} \\
= \frac{1}{1+e^{-x}} \\
= \sigma(x) $$

These functions are all monotonic - their gradients are always non-negative with respect to their inputs. More recent research has introduced a number of *non-monotonic* activation functions, which have a minimum for small negative inputs and tend to zero for large negative inputs. These are the *Gaussian Error Linear Unit*, the *Sigmoid Linear Unit* (SiLU), or *Swish function*, and the *Mish function* (apparently named after Diganta Misra, who devised it).

The GELU activation function is defined as 
$$f(x) = x \frac{1+\mathrm{erf}(x/\sqrt{2 \pi})}{2}$$
where $$\mathrm{erf(x)} = \frac{2}{\sqrt{\pi}} \int_{-\infty}^{x} e^{-x^{2}} dx$$. For this
$$\frac{df}{dx} = \frac{1+\mathrm{erf}(x/\sqrt{2 \pi})}{2} +\frac{e^{-x^{2}/2}}{\sqrt{2 \pi}}$$

The swish function is given by 
$$f(x) = x \sigma(x) = \frac{x}{1+e^{-x}}$$ and its derivative is
$$\frac{df}{dx} = \sigma(x) + x \sigma(x)(1-\sigma(x))$$.

The Mish function is given by 
$$f(x) = x \tanh(\mathrm{softsum}(x)) \\
 = x \frac{e^{x} + 1 - \frac{1}{e^{x} + 1}}{e^{x} + 1 + \frac{1}{e^{x} + 1}} \\
 = x \frac{(e^{x} +1)^{2} -1 }{(e^{x} +1)^{2} + 1}$$
 
 Its derivative is 
 $$\frac{df}{dx} = 4\frac{e^{x}(e^{x}+1)}{((e^{x}+1)^{2} + 1)^{2}}$$
 
 These are quite similar functions. The fact that they are not monotonic gives them the property of being self-regularising - weights and inputs giving rise to large negative values of $x$ will tend to be weakened rather than strengthened during training, thus reducing the the tendency to overfit.
 
 In some circumstances, we may wish to use an activation function that treats both large positive and large negative input values as significant. For this purpose there is a family of activation functions known as *Shrink functions*. The *Hard Shrink* function
 
 $$f(x) = \left\{ \begin{array}{c 1} 0 & \quad \mathrm{if} |x| < 1 \\
 x & \quad \mathrm{otherwise} \end{array} \right. $$ is discontinuous, which may lead to unstable behaviour during training. The *Soft Shrink* function
 $$f(x) = \max(x-1,\min(x+1,0))$$ avoids this problem. However, if we wish to use a smooth function, there is the *Tanh Shrink* function.
 
 $$f(x) = x - \tanh(x)$$
 
 The derivative is
 $$\frac{df}{dx} = (x - f(x))^{2} = \tanh(x)^{2}$$
 
 The activation functions discussed so far apply mainly to hidden layers. For output layers, the activation functions used may be chosen according to the requirements of the problem. The sigmoid function and its generalisation the *Softmax function*
 
 $$p_{i} = \frac{e^{x_{i}}}{\sum_{i} e^{x_{i}}}$$ may be used, while a simple linear output may be suitable for regression.
 
 Both [TensorFlow](https://www.tensorflow.org/api_docs/python/tf/keras/activations) and [PyTorch](https://pytorch.org/docs/stable/nn.html#non-linear-activations-weighted-sum-nonlinearity) provide a wide selection of activation functions.
 
 By [Dr Peter J Bleackley]({% link index.md %})
 
 [Key Algorithms]({% link Key-Algorithms/index.md %})
 
 Components of Neural Networks
 : [The Chain Rule and Backpropogation]({% link Key-Algorithms/chain-rule.md %})
 : *Activation Functions*
 : [Loss Functions]({% link Key-Algorithms/loss-functions.md %})
 : [Gradient Descent]({% link Key-Algorithms/gradient-descent.md %})
 : [Transfer Learning]({% link Key-Algorithms/transfer-learning.md %})
 : [Tokenizers]({% link Key-Algorithms/tokenizers.md %})
  
