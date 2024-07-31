title: Key Algorithms - Transfer Learning

#Transfer Learning

As mentioned in the article about [Multi Layer Perceptron]({% link Key-Algorithms/multi-layer-perceptron.md %}), neural networks need a lot of data and computing time to train. This is especially true for the models currently used in computer vision and natural language processing - large language models have billions of weights to train, and are typically trained on resources like [the Common Crawl](https://commoncrawl.org/) or [the Pile](https://pile.eleuther.ai/}), which contain petabytes of data. 

Clearly, very few organisations have the computing resources to train such a model from scratch. So, if we need a large model for a specific task of our own, what do we do? The usual approach is known as *Transfer Learning*, and aims to transfer information previously learned by a model (such as a statistical model of a language) to a new task. This is achieved by the following procedure.

1. Start with a pre-trained *Base Model*. In naturla language processing, this has often been trained on a task such as masked word prediction or next word prediction.
2. Remove the output layer, or *head*.
3. Replace it with an output layer of your own.
4. *Fine tune* the model, by training it with a dataset specific to your task.

One problem that can occur during fine tuning is *Catastrophic Forgetting*. This is when, rather than adapting the model's existing capabilities to the new task, the new training data simply replaces what has previously been learnt, thus losing the capabilities we wanted to apply to the new task. To avoid this, we typically use a small *learning rate* (the constant by which the gradient is multiplied when adjusting the model weights), which is typically around $10^{-5}$ for NLP models. It is also common to use a *learning rate scheduler* to gradually decrease the learning rate over the course of training.

Transfer learning not only makes training large models more tractable, it also mitigates the energy costs and carbon footprint of doing so.

For [QARAC]({% link QARAC/index.md %}) I am planning to use [RoBERTa](https://huggingface.co/roberta-base) base models, and fine tune them on four different training tasks. The training for these tasks will be done in parallel, since training sequentially would increase the risk of catastrophic forgetting.

[HuggingFace](https://huggingface.co) hosts a wide selection of both base models and fine tuned models suitable for a variety of AI tasks.

By [Dr Peter J Bleackley]({% link index.md %})
 
 [Key Algorithms]({% link Key-Algoritbhms/index.md %})
 
 Components of Neural Networks
 : [The Chain Rule and Backpropogation]({% link Key-Algorithms/chain-rule.md %})
 : [Activation Functions]({% link Key-Algorithms/activation-functions %})
 : [Loss Functions]({% link Key-Algorithms/loss-functions.md %})
 : [Gradient Descent]({% link Key-Algorithms/gradient-descent.md})
 : *Transfer Learning*
 : [Tokenizers]({% link Key-Algorithms/tokenizers.md %})
