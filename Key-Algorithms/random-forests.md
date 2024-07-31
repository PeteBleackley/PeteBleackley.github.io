title: Key Algorithms - Random Forests

#Random Forests

We have mentioned clasification problems in a number of previous articles, and shown how they can be approached with [Bayes' Theorem]({% link Key-Algorithms/bayes-theorem.md %}), [Logistic Regression]({% link Key-Algorithms/logistic-regression.md %}) and by extension, neural networks. This week we'll examine a different method, based on *Decision Trees*.

A decision tree can be thought of a set of nested if/else statements. It can be fit by the following procedure.

1. Find the variable that correlates most strongly with the target variable.
2. Find the set of thresholds against that variable that comes closest to splitting the data into nodes that correspond to the target classes.
3. Repeat this for each of the nodes you have split the data into, until each *leaf node* contains a single class.

However, this is prone to *overfitting*, whereby the model fits every detail of the training data but does not generalise well when classifying new data. In effect, it fits the noise as well as the signal.

*Random Forests* is an algorithm that addresses this problem. As the word forest implies, it fits a large number (typically 100) of decision trees to the training data. Each, however, is trained only on a subset of the training data and with a subset of the variables. These subsets are chosen randomly for each tree in the forest.

While each individual tree in the forest will tend to overfit, the fact that they were all fit against different subsets of the data and variables will mean that the errors they make on new data will not be correlated. Therefore a majority vote of the trees provides a much more robust classifier than any individual tree would. It is also possible to take account of uncertainty in the classification by reporting the number of individual trees that voted for each class - in Bayesian terms, this coresponds to $P(H \mid O)$. Algorithms that combine the results of multiple classifiers in this way are known as *ensemble methods*.

If the target variable is continuous, Random Forests can also be used for regression. In this case, the fitting of the decision trees terminates when the variance of the samples in each leaf node fall below a certain threshold. The prediction is then the mean of the predictions from the individual trees.

Random Forests tend to give better results than Logistic Regression when the target classes are unbalanced, and the algorithm is noted for having a high success rate in [Kaggle](https://kaggle.com) competitions. In [The Grammar of Truth and Lies]({% link Data-Science-Notebooks/grammar-of-truth-and-lies.md %}) I found it gave good results in using grammatical features to classify Fake News.

by [Dr Peter J Bleackley]({% link index.md %})

[Key Algorithms]({% link Key-Algorithms/index.md %})

Simple Supervised Learning Models
: [Logistic Regression]({% link Key-Algorithms/logistic-regression.md %})
: [Linear Regression]({% link Key-Algorithms/linear-regression.md %})
: *Random Forests*
