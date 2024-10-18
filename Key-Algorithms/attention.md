---
title: Key Algorithms - The Attention Mechanism
---

{% include maths.html %}

# The Attention Mechanism

Natural Language Processing systems need to take account the fact that the meaning of a word is dependent on the context in which it occurs. Earlier generations of NLP systems addressed this is various ways. The simplest was to group words into bigrams and trigrams before performing [Latent Semantic Indexing]({% link Key-Algorithms/latent-semantic-indexing.md %}). Another approach was to map the words to an unambiguous ontology using Word Sense Disambiguation, as I did in my work at [True 212]({% link Portfolio/true-212.md %}) using the [Viterbi Algorithm]({% link Key-Algorithms/viterbi-algorithm.md %}). More recently [Recurrent Neural Networks]({% link Key-Algorithms/recurrent-neural-networks.md %}) have been used.

However, the current generation of NLP models are mainly based on *The Attention Mechanism*. Given an $n \times m$ matrix $\mathbf{X}$ which represents a *context window* containing $n$ tokens, each represented by an $m$ dimensional vector, this calculates the context-dependent word vectors as 
$$\mathbf{Y} = \mathbf{A} \cdot \mathbf{V}$$

Where $\mathbf{A}$ is an *attention matrix*, where $A_{ij}$ represents the significance of the $j$th token to the meaning of the $i$th, and $\mathbf{V}$ is a linear projection of the token vectors 
$$\mathbf{V} = \mathbf{X} \cdot \mathbf{W}_{V}$$, known as the *value*.

The attention mechanism takes advantage of the fact that matrix multiplications can be executed efficiently in hardware, thus making it possible to calculate contextual word vectors over a large context window with fewer calculations than recurrent neural networks would require.

To calculate the attention matrix, we first calculate a *Score Function*
$$\mathbf{S} = f(\mathbf{Q},\mathbf{K})$$
where $\mathbf{Q}$ and $\mathbf{K}$ are $n \times d$ linear projections of the context window
$$\mathbf{Q} = \mathbf{X} \cdot \mathbf{W}_{Q}$$
$$\mathbf{K} = \mathbf{X} \cdot \mathbf{W}_{K}$$
known as the *query* and the *key* repsectively. The names *query*, *key* and *value* are based on an analogy with databases, but aren't really significant. The attention matrix is then calculated by applying the [softmax function]({% link Key-Algorithms/logistic-regression.md %}) to the score matrix

$$A_{ij} = \frac{e^{S_{ij}}}{\sum_{j} e^{S_{ij}}}$$

The most commonly-used scoring function is the *Scaled Dot Product Attention*
$$\mathbf{S} = \frac{\mathbf{Q} \cdot \mathbf{K}^{T}}{\sqrt{d}}$$

The scaling is done to prevent the softmax function from saturating and selecting a single token as the sole contributor to the meaning.

Other scoring functions include *Additive Attention*
$$S_{ij} = \vec{v} \cdot \tanh( \mathbf{W}_{1} \cdot Q_{i} + \mathbf{W}_{2} K_{j})$$
where $\vec{v}$, $\mathbf{W}\_{1}$ and $W\_{2}$ are learnable parameters, and *General Attention*
$$\mathbf{S} = \mathbf{Q} \cdot \mathbf{W}_{a} \mathbf{K}^{T}$$ in which the scaling factor used in scaled dot product attention is replaced by a learnable $d \times d$ weight matrix $\mathbf{W}_{a}$$.

For causal language models, where the task is to predict the next word in a sequence, the contextual word vector for each word should depend only on itself and preceding words. We then have the constraint that $A_{ij} = 0$ when $j>i$.

Large language models usually implement *Multi-Head Attention*. In this, several attention layers are applied in parallel to the same input, their outputs concatenated, and the concatenated output fed to a final linear projection to reduce it to the required dimension. This allows each head to detect different relationships between tokens.

Attention layers were originally used to allign inputs with outputs in LSTM-based machine translation systems, in order to account for differing word orders between the source and target languages. However, the paper [Attention is All You Need](https://arxiv.org/abs/1706.03762) introduced the *Transformer Architecture*, in which the model consists entirely of alternating attention and feed-forward layers. Most Large Language Models are based on this.

Attention can also be applied globally to pool token vectors into a single vector representing a document. In [QARAC]({% link QARAC/index.md %}) I use a [Global Attention Pooling Head](https://github.com/PeteBleackley/QARAC/blob/main/qarac/models/layers/GlobalAttentionPoolingHead.py) to do this. In this, the attention for each token is the [cosine similarity]({% link Key-Algorithms/metrics.md %}) between a *local projection* of its input token vector and a *global projection* of the sum of the input vectors. I used cosine similarity here because my aim of mapping logical reasoning to vector arithmetic requires the model to be able to understand negation.

by [Dr Peter J Bleackley]({% link index.md %})

[Key Algorithms]({% link Key-Algorithms/index.md %})

Neural Network Architectures
: [Multilayer Perceptron]({% link Key-Algorithms/multi-layer-perceptron.md %})
: [Convolutional Networks]({% link Key-Algorithms/convolutional-networks.md %})
: [Recurrent Neural Networks]({% link Key-Algorithms/recurrent-neural-networks.md %})
: *The Attention Mechanism*
: [Transformers]({% link Key-Algorithms/transformers.md %})
