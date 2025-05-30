---
title: Key Algorithms - Transformers
---
{% include maths.html %}

# Transformers

In the article on the [Attention Mechanism]({% link Key-Algorithms/attention.md %}) mentioned the *Transformer Architecture*, which is the basis of Large Language Models. In this article, we examine transformers in detail.

A transformer model, like most NLP models, begins with a *tokenizer*, which converts a string input into a sequence of integers, each representing a particular word, subword or symbol stored in the model's vocabulary. If the sequence is shorter than the model's context window, it may be padded to the required length.

An *embedding* is then used to supply an initial vector for each token. The embedding can be seen a $w \times m$ matrix $\mathbf{E}$, where $w$ is the vocabulary size and $m$ the vector width. If the output of the tokenizer is represented by an vector $\vec{T}$ of integers of size $n$, the embedding produces an $n \times m$ matrix $\mathbf{X}$, where
$$\vec{X}_{i} = \vec{E}_{T_{i}}$$

It is desirable that the model should be able to take the position of words into account. For this reason a *Positional Encoding* is added to the initial vectors. This is an $n \times m$ matrix $\mathbf{P}$ where 
$$P_{i} = f(i)$$
Typically used is a sinusoidal positional encoding, where for each $k$ in the range $0 \le k < m/2$
$$P_{i,2k} = \sin\left(\frac{k}{n^{2 i /m}}\right)$$ and
$$P_{i,2k+1} = \cos\left(\frac{k}{n^{2 i / m}}\right)$$

However, more recent transformer models often use a *Rotary Positional Encoding*. In this, we have an $m \times m$ matrix $\mathbf{R}$ which defines a rotation in $m$ dimensional space, and each element of $\mathbf{P}$ is calculated as
$$\vec{P}_{i} = \mathbf{R} \vec{P}_{i-1}$$

This allows both absolute and relative positions to be taken into account.

After this, the vectors are fed to a series of *Transformer Blocks*. The first element of each transformer block is an attention layer. The output of the attention layer is then added to its input, so as to avoid vanishing gradients. This then undergoes *Layer Normalisation*

$$\mathbf{X}^{\prime} =  \gamma \frac{\mathbf{X}-\mu}{\sigma} + \beta $$
Where 
$$\mu = \frac{\sum_{i} \sum_{j} X_{ij}}{n \times m}$$

$$\sigma = \sqrt{\frac{\sum_{i} \sum_{j} (X_{ij} - \mu)^{2}}{n \times m}}$$
and $\gamma$ and $\beta$ are learnable parameters

The normalised outputs are then fed to a feedforward layer. Early transformer models generally used a ReLU [activation function]({% link Key-Algorithms/activation-functions.md %}), but more recent models tend to use one of the non-monotonic functions, such as the swish function.

The output of the feedforward layer is added to its input, and layer normalisation applied again. The output of the second layer normalisation is the output of the transformer block.

Several transformer blocks are stacked together, each processing the output of the previous one and adding more context to the representation of each token. Finally, a *head* layer is applied - this is typically a [logistic regression]({% link Key-Algorithms/logistic-regression.md %}) layer that makes the necessary predictions.

There are three types of transformer models. *Encoder* models use bidirectional attention, where the full context window is taken into account for every token. These are typically trained on a *masked word prediction* task, where some words of the input are masked and the model is trained to predict them. [RoBERTa](https://huggingface.co/docs/transformers/model_doc/roberta) is an example of an encoder model.

*Decoder* models use *masked attention*, where each token's context depends only on itself and previous tokens. These are used for *causal language modelling*, and are typically trained on *next word prediction*. *Generative* or *autoregressive* models synthesize text by iteratively adding each predicted word back into their inputs. Several of the best known LLMs currently, such as the GPT models, [Mistral](https://huggingface.co/mistralai/Mistral-7B-Instruct-v0.3) and [LLaMa](https://huggingface.co/meta-llama/Meta-Llama-3-8B) are decoder models.

*Encoder-decoder* models are used for sequence-to-sequence modelling tasks such as machine translation. They consist of an encoder model and a decoder model coupled by *Cross Attention*, whereby the output of each transformer block in the encoder model is prepended to the input of the corresponding transformer block in the decoder model. [T5](https://huggingface.co/google/t5-v1_1-xxl) is an example of such a model.

Due to their large size and complexity, transformer models are usually specialised to particular tasks by [Transfer Learning]({% link Key-Algorithms/transfer-learning.md %})

by [Dr Peter J Bleackley]({% link index.md %})

[Key Algorithms]({% link Key-Algorithms/index.md %})

Neural Network Architectures
: [Multilayer Perceptron]({% link Key-Algorithms/multi-layer-perceptron.md %})
: [Convolutional Networks]({% link Key-Algorithms/convolutional-networks.md %})
: [Recurrent Neural Networks]({% link Key-Algorithms/recurrent-neural-networks.md %})
: [The Attention Mechanism]({% link Key-Algorithms/attention.md %})
: *Transformers*
