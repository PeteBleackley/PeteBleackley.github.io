---
title: Key Algorithms - The Viterbi Algorithm
---

{% include maths.html %}

# The Viterbi Algorithm

If we have a sequence of events $X_{0},X_{1}...X_{t}$ generated by a [Hidden Markov Model]({% link Key-Algorithms/hidden-markov-models.md %}). One thing we may wish to do is infer the maximum likelihood sequence of hidden states $S_{0},S_{1}...S_{t}$ that gave rise for this. A useful technique for this is the *Viterbi Algorithm*.

The Viterbi algorithm represents the possible paths through the sequence of hidden states as a graphical model called a *trellis*. The possible hidden states at each time step are represented by nodes, with edges representing the transitions between them.

At each time step $t$, we start by calculating the probabilities of the hidden states given the observation at that time step, $P(S\_{i,t} \mid X\_{t})$ and place corresponding nodes on the trellis. We then find the maximum likelihood predecessor for each node

$$\texttt{argmax} \left( P(S_{j,t-1}) P(S_{j,t-1} \mid S_{i,t}) \right)$$

and connect an edge from it to its successor. Any nodes at $t-1$ that have no outgoing edges are then deleted, along with their incoming edge, and this is repeated at each previous time step until no more nodes can be deleted. Then, working forwards through the trellis from the first step at which nodes were deleted, we recalculate the probabilities at each timeslice as 

$$P^{\prime}(S_{i,t}) = \frac{P(S_{j,t-1}) P(S_{i,t} \mid S_{j,t-1}) P(X_{i} \mid S_{i})}{\sum_{i} P(S_{j,t-1}) P(S_{i,t} \mid S_{j,t-1}) P(X_{i} \mid S_{i})}$$

where $S_{i,t}$ are the remaining states at time $t$ and $S_{j,t-1}$ is the maximum likelihood predecessor of each state. 

At the end of the sequence, we may select the maximum likelihood final state $\texttt{argmax} P(S_{i,t})$. The path leading to it is then the maximum likelihood sequence of states given the observations. The Viterbi Algorithm is particularly suitable for real-time applications, as any time step where the number of possible states falls to 1 may be output immediately and removed from the trellis, which in turn reduces memory requirements and computation time.

I first encountered the Viterbi algorithm in the context of error-correcting codes for digital television. The sequence of bits to be transmitted in a digital TV signal can be protected against errors by interspersing it with extra bits derived from a *convolutional code* - this is a binary function of a number of previous bits. This converts the transmitted sequence from an apparently random sequence (due to data compression) to a Markov process. At the receiving side, we treat the received bitstream (which inevitably contains errors) as the observations and the transmitted bitstream as the hidden states, using the Viterbi algorithm to recover it.

I later used the Viterbi Algorithm for [Word Sense Disambiguation]({% link Portfolio/true-212.md %}). In this application, the observations were words and the hidden states were [WordNet](https://wordnet.princeton.edu/ %}) word senses. There were a few complications to take into account - function words, out-of-vocabulary words, multi-word expressions, proper names - but it achieved 70% accuracy, which was described to me as "state of the art".

It's this flexibility and applicability to a range of different problems that makes the Viterbi Algorithm one of my favourite algorithms.

by [Dr Peter J Bleackley]({% link index.md %})

[Key Algorithms]({% link Key-Algorithms/index.md %})

Bayesian Models
: [Bayes' Theorem]({% link Key-Algorithms/bayes-theorem.md %})
: [Hidden Markov Models]({% link Key-Algorithms/hidden-markov-models.md %})
: *The Viterbi Algorithm*
: [Markov Chain Monte Carlo]({% link Key-Algorithms/markov-chain-monte-carlo.md %})
