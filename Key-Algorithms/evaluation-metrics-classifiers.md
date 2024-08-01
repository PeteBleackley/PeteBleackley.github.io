---
title: Key Algorithms - Evaluation Metrics for Classifiers
---

{% include maths.html %}

# Evaluation Metrics for Classifiers

One vitally important task in any data science project is to assess how well the model performs. Various metrics are available for doing this, and each has its own advantages and disadvantages.
This is a large topic, so we will seperate it into metrics suitable for classifiers and those suitable for regression.

A detailed description of the performance of a classifier model is given by the *Confusion Matrix* $\mathbf{C}$, where $C_{ij}$ is the number of instances of class $i$ that are predicted to belong to class $j$. This is useful for visualising the peerformance of the classifier, and the metrics discussed below can be calculated from it

Consider a binary classification problem. We may classify the results in our test dataset as True Positives, True Negatives, False Positives and False Negatives. The number of each of these is denoted $\mathrm{TP} = C_{1,1}$, $\mathrm{TN} = C_{0,0}$, $\mathrm{FP} = C_{0,1}$ and $\mathrm{FN} = C_{1,0}$ respectively.

The *Precision* of the classifier is the probability that an item predicted to be true is actually true. This is given by 
$$ \mathrm{Pr} = \frac{\mathrm{TP}}{\mathrm{TP} +\mathrm{FP}}$$
In Bayesian terms, if the predicted class is $p$ and the actual class is $a$, 
$$\mathrm{Pr} = P(a=\mathsf{True} \mid p=\mathsf{True})$$

The *Recall* of the classifier is the probability that a true item is predicted to te true. This is given by 
$$\mathbf{R} = P(p=\mathsf{True} \mid a=\mathsf{True}) = \frac{\mathrm{TP}}{\mathrm{TP} + \mathrm{FN}} $$

Which of these is more informative depends on the application. In [The Grammar of Truth and Lies]({% link Data-Science-Notebooks/grammar-of-truth-and-lies.md %}), my initial approach gave 100% Recall. However, since I had designated *True* to indicate a reliable article and *False* to indicate fake news, Precision was a more important measure of the model's ability to discriminate fact from fiction.

The F1 score is a metric that seeks to balance Precision and Recall. It is defined as the harmonic mean of them.

$$F_{1} = \frac{2}{1/\mathrm{Pr} + 1/\mathrm{Re}} = \frac{2 \mathrm{Pr} \mathrm{R}}{\mathrm{Pr} + \mathrm{R}} = \frac{2 \mathrm{TP}}{2 \mathrm{TP} + \mathrm{FP} + \mathrm{FN}}$$

This measures similarity between the set of items predicted to be true and those that actually are true, but is not easy to interpret in terms of a Bayesian probability.

The *Accuracy* of the model is the probability that it predicts the correct class.
$$A = P(p=a) = \frac{\mathrm{TP} +\mathrm{TN}}{\mathrm{TP} +\mathrm{TN} + \mathrm{FP} +\mathrm{FN}}$$
This is intuitive to interpret and, unlike the metrics discussed above, takes the true negatives into account. However, it becomes uninformative if classes are strongly imbalanced. For example, if we wish to predict whether or not a user will click on a given advertisement, we can achieve at least 99% accuracy by predicting *No* all the time. We therefore need metrics that correct for class imbalance.

*Cohen's Kappa* is a measure of how much better a classifier is than guesswork. If we guessed the class of an item without information, our best strategy would be to pick the maxumum-likelihood class every time, and this would give us a success rate of $P_{\mathrm{max}}$. We can then define
$$\kappa = 1 - \frac{1 - A}{1 - P_{\mathrm{max}}}$$

*Matthew's Correlation Coefficient* is the [Pearson Correlation Coefficient]({% link Key-Algorithms/metrics.md %}) between the actual and predicted classes. It is calculated as

$$\phi = \frac{\mathrm{TP} \mathrm{TN} - \mathrm{FP} \mathrm{FN}}{\sqrt{(\mathrm{TP} + \mathrm{FP})(\mathrm{TP} + \mathrm{FN})(\mathrm{TN} + \mathrm{FP})(\mathrm{TN} + \mathrm{FN})}}
$$

Accuracy and Cohen's Kappa can be extended to the multiclass case in the obvious way. It is not trivial to do this for Precision and Recall. However, we can define them on a per-class basis. 
$$\mathrm{Pr}_{i} = \frac{C_{ii}}{\sum_{j} C_{ji}}$$
$$\mathrm{R}_{i} = \frac{C_{ii}}{\sum_{j} C_{ij}}$$

[Evidently AI](https://www.evidentlyai.com/classification-metrics/multi-class-metrics) suggests three methods for calculating overall precision and recall scores for calculating overall precision and recall scores in a multiclass problem. *Macro averaging* simple calculates the mean of precision and recall across all classes.
$$\mathrm{Pr} = \frac{\sum_{i} \mathrm{Pr}_{i}}{N}$$
$$\mathrm{R} = \frac{\sum_{i} \mathrm{R}_{i}}{N}$$

where $N$ is the number of classes.

*Micro averaging* gives an average of precision and recall across all instances.

$$\mathrm{Pr} = \frac{\sum_{i} C_{ii}}{\sum_{i} \sum_{j} C_{ji}}$$
$$\mathrm{R} = \frac{\sum_{j} C_{ii}}{\sum_{i} \sum_{j} C_{ij}}$$

These are equivalent, as a false negative for one class is a false positive for another, so while finer grained in one way, micro averaging loses information in another.

The third possibility is *weighted averaging*. While macro averaging gives all classes equal weight, wieghted averaging considers their overall prevalence in the data.
$$\mathrm{Pr} = \frac{\sum_{i} \left( C_{ii} \sum_{j} C_{ij} \right)}{\sum_{i} \left( \sum_{j} C_{ji} \sum_{k} C_{ik} \right)}$$

$$\mathrm{R} = \frac{\sum_{i} \left(C_{ii} \sum_{j} C_{ij} \right)}{\left(\sum_{j} C_{ij} \right)^{2}}$$

To gereralise Matthew's Correlation Coefficient to multiple classes, we first define the following terms
$$t_{k} = \sum_{j} C_{kj}$$ is the number of times class $k$ occurs
$$p_{k} = \sum_{j} C_{jk}$$ is the number of times class $k$ is predicted
$$c = \sum_{k} C_{kk}$$ is the number of correct predictions
$$s =\sum_{i} \sum_{j} C_{ij}$$ is the total number of samples

We then obtain 
$$\phi = \frac{c s - \vec{t} \cdot \vec{p}}{\sqrt{s^{2} - |p|^{2}}\sqrt{s^{2} - |t|^{2}}}$$

Once you have the numbers, of course, it's important to dig deeper and understand what the factors influencing your model's performance are.

by [Dr Peter J Bleackley]({% link index.md %})

[Key Algorithms]({% link Key-Algorithms/index.md %})

Model Evaluation
: *Evaluation Metrics for Classifiers*
: [Evaluation Metrics for Regression]({% link Key-Algorithms/evaluation-metrics-regression.md %})
: [Cross Validation]({% link Key-Algorithms/cross-validation.md %})


