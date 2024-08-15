---
title: Key Algorithms - Multi Layer Perceptron
---
{% include maths.html %}

# Multi Layer Perceptron

One of my aims for this series of articles is to cover the things every data scientist should know. Since neural networks are so ubiquitous in data science, they certainly fit this description.

Neural networks in general learn a function that maps their inputs to the desired outputs. To make learning tractable, that function is built from a number of smaller units. This is meant to mimic the way that processing in the brain in is accomplished by signals being passed between neurons, but compared to the way real neurons work, it's a grossly over-simplfied model. There are many different architectures for combining smaller units to make the overall one, but the most basic is known as a *Multi Layer Perceptron* network.

This consists of an *Input Layer* $\vec{x}$, representing the observations we wish to classify,
$N$ *Hidden Layers* $h_{i}$, where

$$\vec{h_{0}} = f(\mathbf{W}_{0} \cdot \vec{x} + \vec{b_{0}})$$

$$\vec{h_{i+1}} = f(\mathbf{W}_{i} \cdot \vec{h_{i}} + \vec{b_{i}})$$ 

and an *Output Layer*

$$\vec{y} = g(\mathbf{W}_{N} \cdot \vec{h_{N-1}} + \vec{b_{N}})$$

repressenting the model's prediction of the target variable, where 
{% raw %} $\mathbf{W}_{i}$ {% endraw %}
 are weights, {% raw %} $\vec{b_{i}}$ {% endraw %} are biases, 
 and $f$ and $g$ are [activation functions]({% link Key-Algorithms/activation-functions.md %}). In general, the activation functions are non-linear, to allow the model to learn a non-linear function, and differentiable, to allow weights and biases to be learned by [Backpropogation]({% link Key-Algorithms/chain-rule.md %}). While $f$ is usually the same for all hidden layers, $g$ is chosen according to the outputs required - for example, a [softmax function]({% link Key-Algorithms/logistic-regression.md %}) may be used for classification problems.

Multi Layer Perceptron networks are powerful algorithms, but they do have some disadvantages. Generally, using more layers allows them to make better predictions - indeed, it when the computational power to train and run networks with many layers (*deep learning*) became available that neural networks first became viable for mainstream use. However, the number of hidden layers and the width of each layer are hyperparameters that may need some experimentation to choose correctly. The complexity of the models means that a lot of data and computing time is needed to train them, they may be prone to overfitting and it is hard to explain their results.

Having said that, if the problem to be solved is sufficiently complex, neural networks might well give better results than anything else. I consider it best practice to thoroughly investigate the data and see whether a simpler model will give good results first, and turn to neural networks only when necessary.

by [Dr Peter J Bleackley]({% link index.md %})

[Key Algorithms]({% link Key-Algorithms/index.md %})

Neural Network Architectures
: *Multilayer Perceptron*
: [Convolutional Networks]({% link Key-Algorithms/convolutional-networks.md %})
: [Recurrent Neural Networks]({% link Key-Algorithms/recurrent-neural-networks.md %})
: [The Attention Mechanism]({% link Key-Algorithms/attention.md %})
: [Transformers]({% link Key-Algorithms/transformers.md %})
