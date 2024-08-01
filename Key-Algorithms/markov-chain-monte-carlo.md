---
layout: default
title: Key Algorithms -  Markov Chain Monte Carlo
---

# Markov Chain Monte Carlo

In our previous discussions of [Bayes' theorem]({% link Key-Algorithms/bayes-theorem.md %}) we have assumed that the probability distributions involved are of discrete variables. However, in many cases we wish to deal with continuous variables. In this case, Bayes' Theorem becomes

$$P(H \mid O) = \frac{P(H) P(O \mid H)}{\int P(H) P(O \mid H) dH}$$

Unfortunately, for many distributions we may be interested in (including the ubiquitous normal distribution), the integral involved is intractible. The problem only gets worse in complex models, especially where we distributions may have multiple parameters. Some distribuitions have a *conjugate prior*, where the posterior distribution is of the same form as the prior distribution and may be obtained by an appropriate adjustment of parameters, but this is not always the case, and we need a numerical method that is more generally applicable.

The method we use is called *Markov Chain Monte Carlo* because it uses random samples from Markov chains to explore the parameter space of the distribution. There are a number of variations of this, so for the sake of illustration, we will select a particular variant, the *Metropolis-Hastings algorithm*, as the basis of further discussion.

We start with a Markov chain $P(H^{\prime} \mod H)$ that, given a sample hypothesis $H$ generates a nearby hypothesis $H^{\prime}$. At timestep $t=0$, we generate a set of samples $H_{i,0}$ from the prior distribution. Then at each timestep $t$, we generate a set of alternative hypotheses $H^{\prime}_{i,t}$ from the Markov chain given $H_{i,t}$. For each pair of hypotheses, we then calculate an acceptance probability

$$ A(H^{\prime}_{i,t},H_{i,t}) = \min \left( 1, \frac{P(O \mid H^{\prime}_{i,t}) P(H^{\prime}_{i,t}) P(H_{i,t} \mid H^{\prime}_{i,t})}{P(O \mid H_{i,t}) P(H_{i,t}) P(H^{\prime}_{i,t}) \mid H_{i,t}} \right) $$

We then generate a set of samples $S_{i}$ from a uniform distribution between 0 and 1, and update the samples as

$$H_{i,t+1} = \left\{  \begin{array}{c 1} H^{\prime}_{i,t} & \quad \textrm{if } S_{i} \leq A(H^{\prime}_{i,t},H_{i,t}) \\ H_{i,t} & \quad \textrm{otherwise} \end{array} \right.$$

Provided that the model and the choice of priors is suitable for the data being modelled, over sufficient steps, the distiribution of $H_{i,t}$ will converge to $P(H|O)$. We can envision this as each sample exploring the nearby regions of the distribution and preferring to move towards regions of higher likelihood.

Markov Chain Monte Carlo is implemented in the [PyMC](https://www.pymc.io/) library, which provides a comprehensive toolkit for probabilistic modelling. 

by [Dr Peter J Bleackley]({% link index.md %})

[Key Algorithms]({% link Key-Algorithms/index.md %})

Bayesian Models
: [Bayes' Theorem]({% link Key-Algorithms/bayes-theorem.md %})
: [Hidden Markov Models]({% link Key-Algorithms/hidden-markov-models.md %})
: [The Viterbi Algorithm]({% link Key-Algorithms/viterbi-algorithm.md %})
: Markov Chain Monte Carlo*
