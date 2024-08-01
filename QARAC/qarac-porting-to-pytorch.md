---
layout: default
title: "QARAC: Porting to PyTorch"
---

# QARAC: Porting to PyTorch

Most of my previous work with neural networks has been based on the [Keras](https://keras.io) library, so in implementing QARAC, I initially used what I was most familiar with. Some lower-level parts of the algorithm were implemented in [TensorFlow](https://tensorflow.org), which for some years has been the default backend to Keras (however, it is now possible to use Keras with a choice of backends again). 

I decided to to some local testing before committing large amounts of compute time to training the models, but when I did so, I got the following warning.

```
WARNING:tensorflow:Gradients do not exist for variables ['tf_roberta_model/roberta/pooler/dense/kernel:0', 'tf_roberta_model/roberta/pooler/dense/bias:0', 'qarac_trainer_model/qarac_encoder_model/global_attention_pooling_head/local projection:0', 'qarac_trainer_model/qarac_encoder_model_1/global_attention_pooling_head_1/local projection:0', 'tf_roberta_model_1/roberta/pooler/dense/kernel:0', 'tf_roberta_model_1/roberta/pooler/dense/bias:0'] when minimizing the loss. If you're using `model.compile()`, did you forget to provide a `loss` argument?
WARNING:tensorflow:Gradients do not exist for variables ['tf_roberta_model/roberta/pooler/dense/kernel:0', 'tf_roberta_model/roberta/pooler/dense/bias:0', 'qarac_trainer_model/qarac_encoder_model/global_attention_pooling_head/local projection:0', 'qarac_trainer_model/qarac_encoder_model_1/global_attention_pooling_head_1/local projection:0', 'tf_roberta_model_1/roberta/pooler/dense/kernel:0', 'tf_roberta_model_1/roberta/pooler/dense/bias:0'] when minimizing the loss. If you're using `model.compile()`, did you forget to provide a `loss` argument?
```

It looks like the [Training Model]({% link QARAC/qarac-models-and-datasets.md %}) wasn't able to propogate gradients between its constituent models. This seems to be a feature of the architecture of Keras, in which `Model`s are made up of `Layer`s. Layers are designed to be components of a larger model, and so propogate gradients across their inputs, whereas models, which are intended to be complete systems, do not. From Keras's point of view, I was trying to use models as layers, and it didn't like it.

Since [HuggingFace](https://huggingface.co) models are available in both TensorFlow and [PyTorch](https://pytorch.org), so I looked to see if PyTorch would be more suitable for what I wanted to do. I found that PyTorch doesn't make the same distinction that Keras does between layers and models - both are `Module`s, so there would be not problem with propogating gradients between them. The learning curve going from Keras to PyTorch wasn't too steep. The main differences were that the method of a Keras layer that's called `call` is called `forward` in PyTorch, and there's no direct equivalent of a Keras model's `compile` and `fit` methods, so you have to write a training loop. Also, HuggingFace's PyTorch and TensorFlow models aren't exact drop-in replacements for each other, so on occasion adjustments were needed where one wanted a parameter that the other didn't. 

You should learn something new on every project, and that has been one of my key personal goals for QARAC. I didn't envisage that I'd end up learning PyTorch for this project, but the fact that I have done is welcome and will come in useful for future projects. 

There's only one more thing I need before I can train the models, and that's a budget for compute time, or a [community hardware grant](https://huggingface.co/docs/hub/spaces-gpus#community-gpu-grants) from HuggingFace.

by [Dr Peter J Bleackley]({% link index.md %})

[QARAC]({% link QARAC/index.md %})
