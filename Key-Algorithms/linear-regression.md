---
title: Key Algorithms - Linear Regression
---

{% include maths.html %}

# Linear Regression

 For many problems, we wish to fit a function of the form

$$y = m x + c$$

or, for multivariate problems

$$\vec{y} = \mathbf{M} \vec{x} + \vec{c}$$

The simplest method for this is *Ordinary Least Squares*, which chooses the parameters so as to mimimise the mean squared error of the model. This has a closed form solution, but there are disadvantages to using it with multivariate data. Firstly, there is a danger of overfitting, with variables of little importance adding to the complexity of the model, and secondly there is the possibility of dependencies existing between the input variables and thus introducing redundancy into the model. These issues may be addressed by applying [principal component analysis]({% link Key-Algorithms/data-reduction.md %}) to the input data, but this has the disadvantage of making the model less explainable.

There are a number of methods of reducing the complexity of multivariate linear regression models. One of these is *Least Angle Regression* (LARS). This is a method of fitting the model that minimises the number of components used to predict the outputs. Rather than following the gradient of the loss function, it adjust the weights corresponding with the input variable that has the strongest correlation with the residuals at each step of the optimisation. When more than one variable have equally strong correlations with the target residuals, they are increased together in the joint least squares direction. While LARS identifies the most important variables contributing to the prediction, it does not solve the problem of collinearity between variables and is sensitive to noise.

Other methods for preventing overfitting involve adding a *regularisation penalty* to the loss function in the optimisation. For *Lasso regression*, this penalty is the sum of the absolute values of the weights, so the loss function to be optimised is

$$L = \frac{\sum_{i}\left| \vec{y}_i - \left(\mathbf{M} \vec{x}_{i} + \vec{c} \right) \right|^{2}}{2 N} + \alpha \sum_{j} \sum_{k} |M_{jk}|$$
where $N$ is the number of samples and $\alpha$ is a hyperparameter.

For *Ridge regression*, the penalty term is the sum of the squares of the model weights, hence the loss function is 

$$L = \frac{\sum_{i}\left| \vec{y}_i - \left(\mathbf{M} \vec{x}_{i} + \vec{c} \right) \right|^{2}}{2 N} + \alpha \sum_{j} \sum_{k} M_{jk}^{2}$$

Lasso regression favours sparse models (that is, those with fewer non-zero weights), whereas ridge regression favours generally small weights.

These methods can be combined. *Lasso LARS* applies the Lasso regularisation penalty to LARS, which reduces LARS vulnerability to collinearity and noise. In [Clustering Proteins in Breast Cancer Patients]({% link Data-Science-Notebooks/clustering-proteins.md %}) I used this method to fit numerical variables related to the progress of cancer to measures of activity in clusters of proteins. This method was chosen because I wished to assess which protein clusters were strong predictors.

*ElasticNet* combines the Lasso and Ridge regression methods, optimising the loss function

$$L = \frac{\sum_{i}\left| \vec{y}_i - \left(\mathbf{M} \vec{x}_{i} + \vec{c} \right) \right|^{2}}{2 N} + \alpha \left( \rho \sum_{j} \sum_{k} |M_{jk}| + (1 - \rho) \sum_{j} \sum_{k} M_{jk}^{2} \right)$$

where $\rho$ is another hyperparameter, ranging from 0 to 1, which determines the relative importance of the two regularisation penalties.

These algorithms and a number of related ones, are implemented in [Scikit-Learn](https://scikit-learn.org/stable/modules/linear_model.html)

by [Dr Peter J Bleackley]({% link index.md %})

[Key Algorithms]({% link Key-Algorithms/index.md %})

Simple Supervised Learning Models
: [Logistic Regression]({% link Key-Algorithms/logistic-regression.md %})
: *Linear Regression*
: [Random Forests]({% link Key-Algorithms/random-forests.md %})
