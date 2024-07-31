title: Key Algorithms - Bayes' Theorem

# Bayes Theorem 

If you look at [my LinkedIn profile](https://www.linkedin.com/in/peterjbleackley), you'll see that the banner shows the formula $$P(H \mid O) = \frac{P(H) P(O \mid H)}{P(O)}$$

This is a foundational rule for calculating conditional probabilities, known a *Bayes' Theorem*, after the Reverend Thomas Bayes, who first proposed it. It may be read as *the probability of a hypothesis given some observations is equal to the prior probability of the hypothesis multiplies by the probability of the observations given that hypothesis, and divided by the probability of the observations*. 

To illustrate this, consider a family where the father has rhesus-positive blood and the mother has rhesus-negative blood. Rhesus-positive is a dominant trait - the father might have one or two copies of the Rh+ gene, whereas rhesus-negative is recessive - the mother must have two copies of the Rh- gene.

Let $H$ be the probability that the father has 2 copies of the RH+ gene. Without further information, 1/2 is the best estimate for this. If the family's first child is rhesus-positive, the probability of this is $P(O \mid H) = 1$ if the father has two copies of the Rh+ gene and $P(O \mid ¬H) = \frac{1}{2}$ if he has 1 copy. In general the overall probability of the observations give a set of hypotheses $H_{i}$ is given by
$$P(O) = \sum_{i} H_{i} P(O \mid H_{i})$$, since the posterior probabilities of all hypotheses must sum to 1. Therefore, we can update the probability of the father having two copies of the Rh+ gene as
$$P(H \mid O) = \frac{P(H) P(O \mid H)}{(P(H) P(O \mid H) + P(¬H) P(O \mid ¬H)} = \frac{\frac{1}{2} \times 1}{\frac{1}{2} \times 1 + \frac{1}{2} \times \frac{1}{2}} = \frac{2}{3}$$

If the family's second child is also rhesus-positive, we can further update our estimate with the new information

$$P(H \mid O) = \frac{P(H) P(O \mid H)}{(P(H) P(O \mid H) + P(¬H) P(O \mid ¬H)} = \frac{\frac{2}{3} \times 1}{\frac{2}{3} \times 1 + \frac{1}{3} \times \frac{1}{2}} = \frac{4}{5}$$

It is easy to see that if we had known both children's blood groups from the outset, and used $P(O \mid ¬H) = \frac{1}{4}$ we could have got the same result.

In data science, we often have to estimate the probability of a hypothesis given some evidence, so Bayes' theorem is a useful thing to have in our toolkit. 

If we need to take observations of several different variables into account, there are two ways we can do it, the first, the *Naive Bayes* approach, treats all the variables as statistically independent, as we did in the above example. While this has the advantage of simplicity, it is only really viable when the problem is sufficiently simple.

For more complex problems, we need to model the dependencies between variables. We do this with a graphical method called a *Bayesian Belief Net*, where each node on a graph represents a variable, and the links represent dependencies between them. Each node then calculates the probability of the variable it represents in terms of the variables it is dependent on. A simple example can be seen in the Data Science Notebook [Is It a Mushroom or Is It a Toadstool?]({% link Data-Science-Notebooks/mushroom.md %}).

For my first AI project, I was asked to chose the best system to implement an automatic diagnostic system. I chose a Bayesian Belief Network on the grounds that it was important for the system to be explainable. Since each node of the Bayesian Belief Newtork represents a meaningful variable, its results are more explainable that those of a neural network, whose nodes are simply steps in a calculation. More recently I used Bayesian models in a project to predict the optimum settings for machine tools, so Bayes' Theorem has followed me throughout my data science career.

by [Dr Peter J Bleackley]({% link index.md %})

[Key Algorithms]({% link Key-Algorithms/index.md %})

Bayesian Models
: *Bayes' Theorem*
: [Hidden Markov Models]({% link Key-Algorithms/hidden-markov-models.md %})
: [The Viterbi Algorithm]({% link Key-Algorithms/viterbi-algorithm.md %})
: [Markov Chain Monte Carlo]({% link Key-Algorithms/markov-chain-monte-carlo.md %})
