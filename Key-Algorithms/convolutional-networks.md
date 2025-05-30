---
title: Key Algorithms - Convolutional Networks
---

{% include maths.html %}

# Convolutional Networks

[Multi Layer Perceptron]({% link Key-Algorithms/multi-layer-perceptron.md %}) networks would not be suitable for computer vision. Consider a fully-connected layer that takes a $1080 \times 1920$ RGB image (this is the resolution of a HDTV image) and outputs $N$ features. This would require $(1080 \times 1920 \times 3 + 1 ) N = 6220801 N$ parameters for the first hidden layer alone. Worse still, it would be unable to generalise - simply shifting the inputs by 1 pixel in any direction would produce a completely different output.

To address this problem, we need to look at how computer vision systems typically worked before deep learning became commonplace. They would start by detecting simple, localised features such as edges, and then build them up into hierarchies of more complex features. Algorithms based on this approach can be found in the [OpenCV](https://opencv.org/) library.

*Convolutional Networks* allow us to make these features learnable. A convolutional layer has a *kernel*, which applies to a $k \times k$ region of its inputs. For computer vision applications  If the previous layer produces $N\_{in}$ features, and this layer produces $N\_{out}$ features, the total number of parameters needed for the kernel is $(k \times k \times N\_{in} + 1) N\_{out}$, a vast saving of complexity - for a $3 \times 3$ kernel applies to an RGB input image, we would require only $28 N\_{out}$ parameters. The kernel is scanned across the inputs, creating $N\_{out}$ features at each position in the image (padding is usually applies at the edges of the image to prevent loss of information). This is the convolution that the algorithm is named after, and it makes the features detected invariant under translation.

Between convolutional layers there is often a *Pooling Layer* which combines the features of adjacent pixels, so as to reduce the size of the layer. These divide their inputs into windows (typically $2 \times 2$) and use an aggregation function to chose one value for each feature to represent the window as a whole. Typically, either the maximum or the average is used. Pooling layers help to regularise the model, and increase invariance against small local distortions of the image. After a number of alternating layers of convolution and pooling, they will reduce the image size to the point where fully connected layers can tractably be used for the final task.

Convolutional layers are not restricted to computer vision problems. While this discussion has mainly addressed 2-dimensional convolutional networks. There are also 1-dimensional convolutional networks, which find application in natural language processing, speech recognition, and time series analysis.

Convolution and pooling layers are implemented in [Keras](https://keras.io/2.16/api/layers/convolution_layers/) and [PyTorch](https://pytorch.org/docs/stable/nn.html#convolution-layers)

by [Dr Peter J Bleackley]({% link index.md %})

[Key Algorithms]({% link Key-Algorithms/index.md %})

Neural Network Architectures
: [Multilayer Perceptron]({% link Key-Algorithms/multi-layer-perceptron.md %})
: *Convolutional Networks*
: [Recurrent Neural Networks]({% link Key-Algorithms/recurrent-neural-networks.md %})
: [The Attention Mechanism]({% link Key-Algorithms/attention.md %})
: [Transformers]({% link Key-Algorithms/transformers.md %})
