---
layout: default
title: Key Algorithms -  Information Theory
---

# Information Theory 

Data science can be described as turning data into infomation. However, we need to know how much information there is to find and where to find it. There are various methods we can use to measure this, which derive from the field of *Information Theory*.

The most basic of these measurements is *entropy*, which was intoduced by Claude Shannon. If a varible has a probability distribution $p_{i}$, the entropy of that variable is given by
$$H = -\sum_{i} p_{i} \log_{2}p_{i}$$
This is the expected number of binary decisions needed to identify a value of the variable, or, if we were to generate a stream of symbols from that distribution, the average number of bits per symbol that would be needed to encode that stream in an optimal lossless compression.
This is useful for identifying which variables are most important. Entropy has its maximum value of $\log_{2} N$, where $N$ is the number of possible values, when the values are evenly distributed, and its minimum value of 0 when one value is a certainty.

We also need to quantify how much information is contained in the relationship between two variables. Suppose that two variables $A$ and $B$ have individual probability distributions $p_{i}$ and $p_{j}$, and a joint probability distribution $p_{ij}$. If the variables are statistically independent, these distributions would satisfy the relationship $p_{ij} = p_{i} p_{j}$. *Mutual information* characterises the deviation from this as
$$\mathrm{MI}(A,B) = \sum_{i} \sum_{j} p_{ij} \log_{2} \frac{p_{ij}}{p_{i} p_{j}}$$
This is the amount of information that knowing the value of one variable will tell you about the other. This can be used for feature selection. Consider two variables $A$ and $B$ and a target variable $T$. If $\textrm{MI}(A,T) > \textrm{MI}(B,T)$ and $\textrm{MI}(A,B) > \textrm{MI}(B,T)$, it is likely that any relationship between $B$ and $T$ is entirely a consequence of their mutual relationship with $A$. Therefore, $B$ can safely be discarded.

In [Is It A Mushroom or Is It A Toadstool]({% link Data-Science-Notebooks/mushroom.md %}) I used mutual information to infer hidden variables when building a [Bayesian Belief Network]({$ link Key-Algorithms/bayes-theorem.md %}).

There are number of information-theory based methods for selecing models. The best known of these, which are closely related to each other are the *Bayesian Information Criterion*

$$\mathrm{BIC} = k \ln n - 2 \ln \hat{L}$$ and the *Akaike Information Criterion*

$$\mathrm{AIC} = 2 ( k- \ln \hat{L} )$$

where $k$ is the number of free parameters in the model, $n$ is the number of data points to which the model is fitted, and $\hat{L}$ is the likelihood of the data under the optimally fitted model. In both cases, a lower value indicates a better model, favouring models that give a high likelihood of the data and penalising more complex models. The main difference between them is that the Bayesian Information Criterion penalises compelexity more heavily, especially for larger datasets.

There are many other uses for information theory in data science, but I'd like finish with one relevant to natural language processing. Marcello Montemurro and Damian Zanette published a paper entitled [Towards the quantification of semantic information in written language](https://arxiv.org/abs/0907.1558) in which they intoduced a technique for using the entropy of word frequency distributions across different parts of a document to identify the most significant words, according to the role they play in its structure. I illustrate this in [The Entropy of Alice in Wonderland]({% link Data-Science-Notebooks/entropy-of-alice-in-wonderland.md %}).

By [Dr Peter J Bleackley]({% link index.md %})
 
 [Key Algorithms]({% link Key-Algorithms/index.md %})
 
 Measurements
: [Similarity and Difference Metrics]({% link Key-Algorithms/metrics.md %})
: [TF-IDF]({% link Key-Algorithms/tf-idf.md %})
: [Data Reduction]({% link Key-Algorithms/data-reduction.md %})
: [Latent Semantic Indexing]({% link Key-Algorithms/latent-semantic-indexing.md %})
: *Information Theory*

