---
layout: default
title: Key Algorithms - Recurrent Neural Networks
---

# Recurrent Neural Networks

*Recurrent Neural Networks* (RNN) are to [neural networks]({% link Key-Algorithms/multi-layer-perceptron.md %}) what [Hidden Markov Models]({% link Key-Algorithms/hidden-markov-models.md %}) are to [Bayesian Models]({% link Key-Algorithms/bayes-theorem.md %}). That is, they are a form of the model that is designed to analyse sequences which evolve over time. While 1-dimensional [Convolutional Networks]({% link Key-Algorithms/convolutional-networks.md %}) can be used to analyse sequences, they are sensitive mainly to short range relationships between samples, whereas recurrent networks aim to capture longer range relationships.

In general, given the input $\vec{x}_{t}$ at timestep $t$, and the previous output $y_{t-1}$
the output $\vec{y}_{t}$ is calculated as
$$\vec{y}_{t} = g(\vec{x}_{t}, \vec{c}_{t}, \vec{y}_{t-1})$$
where  $\vec{c}_{t}$ is a *cell state* vector, which is updated by
$$\vec{c}_{t} = f(\vec{x}_{t}, \vec{c}_{t-1}, \vec{y}_{t-1})$$
$f$ and $g$ are learnable functions. The simplest version is the *Ellman RNN*
$$\vec{y}_{t} = \tanh(\mathbf{W}_{i} \cdot \vec{x}_{t} + \mathbf{W}_{h} \cdot \vec{y}_{t-1})$$
where in general $\mathbf{W}$ and $\vec{b}$ are weight matrices and bias terms respectively. Elsewhere you will see these equations written with two different bias terms, but since they are additive, it is mathematically equivalent (and simpler) to express them as one.

When training a recurrent network, gradients must be [backpropogated]({% link Key-Algorithms/chain-rule.md %}) over a large number of timesteps, which in simple recurrent models leads to the vanishing gradient problem. Currently used recurrent network architectures are designed to mitigate this problem.

The most commonly used RNN architecture is *Long Short Term Memory* (LSTM). An *LSTM cell* consists of a number of *gate* functions, which control how information flows between the input, cell state, and outputs. The *Input Gate* controls how much information flows from the input and the previous output to the cell state. It is 
$$\vec{i}_{t} = \sigma(\mathbf{W}_{ii} \cdot \vec{x}_{t} + \mathbf{W}_{hi} \cdot \vec{y}_{t-1} + \vec{b}_{i})$$
where $\sigma$ is the [logistic function]({% link Key-Algorithms/logistic-regression.md %}).

The *Forget Gate* controls how much information the cell state retains from one timestep to the next. It is given by 

$$\vec{f}_{t} = \sigma(\mathbf{W}_{if} \cdot \vec{x}_{t} + \mathbf{W}_{hf} \cdot \vec{y}_{t-1} + \vec{b}_{f})$$

The *Cell Gate* combines information from the input and the previous output. It is given by

$$\vec{g}_{t} = \tanh(\mathbf{W}_{ig} \cdot \vec{x}_{t} + \mathbf{W}_{hg} \cdot \vec{y}_{t-1} + \vec{b}_{g})$$

The *Output Gate* controls how information flows from the cell state to the output. It is given by 
$$\vec{o}_{t} = \sigma(\mathbf{W}_{io} \cdot \vec{x}_{t} + \mathbf{W}_{ho} \cdot \vec{y}_{t-1} + \vec{b}_{o})$$ 

At each timestep, the cell state is updated by 
$$\vec{c}_{t} = \vec{f}_{t} \odot \vec{c}_{t-1} + \vec{i}_{t} \odot \vec{g}_{t}$$
and the output is calculated as 
$$\vec{y}_{t} = \vec{o}_{t} \odot \tanh(\vec{c}_{t})$$

A variation on this is a *Peephole LSTM*, in which the gate functions also include a term in the previous cell state. 

A simpler alternative to LSTMs is *Gated Recurrent Units* (GRU). This dispenses with the cell state, and uses a simpler set of gates. The *Update Gate* controls whether the output will be primarily based on the previous output or the new input
$$\vec{z}_{t} = \sigma(\mathbf{W}_{iz} \cdot \vec{x}_{t} + \mathbf{W}_{hz} \cdot \vec{y}_{t-1} + \vec{b}_{z})$$
The *Reset Gate* controls how much influence the previous output will have on the calculation of a new candidate output.
$$\vec{r}_{t} = \sigma(\mathbf{W}_{ir} \cdot \vec{x}_{t} + \mathbf{W}_{hr} \cdot \vec{y}_{t-1} + \vec{b}_{r})$$
A *Candidate Ouptut* is calculated as 
$$\vec{h}_{t} = \tanh(\mathbf{W}_{ih} \cdot \vec{x}_{t} + \mathbf{W}_{hh} \cdot (\vec{r}_{t} \odot y_{t-1}) + \vec{b}_{h})$$
The output is then calculated as
$$y_{t} = (\vec{1} - \vec{z}_{t}) \odot \vec{y}_{t-1} + \vec{z} \odot \vec{h}_{t}$$

This can be simplified even further as a *Minimal Gated Unit* (MGU), in which the update and reset gates are combined into a single forget gate.

When the whole sequence to be analysed is known in advance, a *Bidirectional RNN* can be used. This combines the outputs of two RNNs, one of which is working on a reversed copy of the inputs and has its outputs reversed in turn. This means that the output vector produced at each step of the sequence will take the context of the entire sequence into account.

I used LSTMs in my work at [Formisimo]({% link Portfolio/formisimo.md %}) to predict whether users were likely to complete or abandon web forms. Recurrent networks are also used in OCR applications and in NLP - an interesting example is [Flair](https://huggingface.co/flair), which runs a bidirectional LSTM over the characters of its input sequence, and samples the vectors corresponding to the last character of each word in the forward direction and the first character of the word in the backward direction to produce a contextual word vector for each word.

Recurrent networks can be implemented in [Keras](https://keras.io/api/layers/recurrent_layers/) and [PyTorch](https://pytorch.org/docs/stable/nn.html#recurrent-layers).

by [Dr Peter J Bleackley]({% link index.md %})

[Key Algorithms]({% link Key-Algorithms/index.md %})

Neural Network Architectures
: [Multilayer Perceptron]({% link Key-Algorithms/multi-layer-perceptron.md %})
: [Convolutional Networks]({% link Key-Algorithms/convolutional-networks.md %})
: *Recurrent Neural Networks*
: [The Attention Mechanism]({% link Key-Algorithms/attention.md %})
: [Transformers]({% link Key-Algorithms/transformers.md %})
