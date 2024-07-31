title: Key Algorithms - Hidden Markov Models

# Hidden Markov Models

Suppose we wish to analyse a sequence of events $X_{0},X_{1}...X_{t}$. This can be modelled using [Bayes' Theorem]({% link Key-Algorithms/bayes-theorem.md %}) as a *Markov process* $P(X_{t} \mid X_{t-1} )$, *ie* the probability of each event depends on the previous event in the sequence.

If there are $N$ possible values that $X$ can take, the number of transition probabilities between them is $N^{2}$. Such a model would quickly become very large and not very informative. We need a way to make the models more tractable.

To do this, we assume that the probability of each event can be described in terms of a hidden state, $S$, as $P(X_{t} \mid S_{t})$. The states can then be modelled by a Markov process, $P(S_{t} \mid S_{t-1})$. This is known as a *Hidden Markov Model*, since it models a sequence of hidden states with a Markov process. The number of hidden states can be considerable smaller than the number of possible events, and the states can group events into meaningful categories. The model consists of three distributions, the inital state distribution, $P(S_{0})$, the transition probability distribution, $P(S_{t} \mid S_{t-1})$, and the conditional distribution of the events $P(X \mid S)$. 

Starting from the initial state distribution $P(S_{0})$, we can caclulate the posterior distributions of the hidden states at each step $t$ of a sequence by the following method.

1. Calculate the posterior distribution of the hidden state given the observed event $X_{t}$ using Bayes' Theorem
$$P(S_{t} \mid X_{t}) = \frac{P(S_{t}) P(X_{t} \mid  S_{t})}{P(X_{t})}$$
2. Calculate the prior probability of the next state
$$P(S_{t+1}) = P(S_{t+!} \mid S{t}) P(S_{t} \mid {X_t})$$


A concrete example is [Part of Speech Tagging]({% link Data-Science-Notebooks/part-of-speech-tagging.md %}). In this application, the observed events are words and the hidden states are the parts of speech (noun, verb, adjective etc.). This approach is particularly useful when you want the probability of each part of speech for a given word, rather than a single tag. I used this approach in my work at [True 212]({% link Portfolio/true-212.md %}), using my own open source [Hidden Markov Model library](https://pypi.python.org/pypi/Markov), which I had created as a lerning exercise when I first learnt about HMMs. I was pleased to discover that a colleague on that project had also used the library, but I no longer maintain it, as I've learnt a lot since then and if I did any more work on it I'd prefer to restart it from scratch.

by [Dr Peter J Bleackley]({% link index.md %})

[Key Algorithms]({% link Key-Algorithms/index.md %})

Bayesian Models
: [Bayes' Theorem]({% link Key-Algorithms/bayes-theorem.md %})
: *Hidden Markov Models*
: [The Viterbi Algorithm]({% link Key-Algorithms/viterbi-algorithm.md %})
: [Markov Chain Monte Carlo]({% link Key-Algorithms/markov-chain-monte-carlo.md %})
