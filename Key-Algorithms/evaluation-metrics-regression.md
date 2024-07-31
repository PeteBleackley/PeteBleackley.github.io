title: Key Algorithms - Evaluation Metrics for Regression
Author: Dr Peter J Bleackley

# Evaluation Metrics for Regression

In the previous article, we looked at [Evaluation Metrics for Classifiers]({% link Key-Algorithms/evaluation-metrics-classifiers.md %}) which are applicable when we are predicting discrete categories. This time, we'll look at how to evaluate models that predict continuous variables.

Suppose, in our test dataset, we have $N$ data points. We'll designate the predicted values as $f_{i}$ and the actual values as $y_{i}$. One of the most obvious metrics to use is the *mean squared error*

$$\mathrm{MSE} = \frac{\sum_{i} (y_{i} - f_{i})^{2}}{N}$$

This is essentially the variance of the errors. Since the mean squared error is often used as the [loss function]({% link Key-Algorithms/loss-functions.md %}) when fitting a regression model, we can easily compare this metric to the fitting loss to give an indication of how well the model has generalised. However, it can be difficult to interpret, since the scale of the metric is not the same as the original data. We may therefore wish to use the *root mean squared error*

$$\mathrm{RMSE} = \sqrt{\frac{\sum_{i} (y_{i} - f_{i})^{2}}{N}}$$

which is the standard deviation of the errors. However, both these metrics can be sensitive to outliers, because of the squaring of the errors, which effectively gives larger errors higher weight. A metric that is less sensitive to this is the *mean absolute error*

$$\mathrm{MAE} = \frac{\sum_{i} |y_{i} - f_{i}|}{N}$$

This gives the same weight to small errors as to large ones. If we were to chose a constant $f$ that minimises the mean absolute error, it would correspond to the median of $y$.

If we wish to use a metric that is independent of the scale of the data, we can use the *mean absolute percentage error*

$$\mathrm{MAPE} = \frac{1}{N}\sum_{i} \left| \frac{y_{i} - f_{i}}{y_{i}} \right|$$

While this is intuitively easy to understand, it has two disadvangtages. One is that it gives lower errors when the predicted valuea are too high than when they are two low, and the other is that it can diverge if any of the values of $y_{i}$ are close to zero. There are a number of approaches to mitigating these disadvantages. The *weighted mean absolute percetage error*

$$\mathrm{wMAPE} = \frac{\sum_{i}|y_{i} - f_{i}|}{\sum_{i}|y_{i}|}$$ 
is robust against divergence, because it scales the errors by the mean absolute value of the true values, rather than the individual true values.

The *symmetrical mean average percentage error*
$$\mathrm{sMAPE} = \frac{100}{N} \frac{|y_{i} - f_{i}|}{|y_{i}| + |f_{i}|}$$
is bounded between 0% and 100%. When $y_{i}$ and $f_{i}$ are both 0, the datapoint's percentage error is taken to be 0.

The *mean absolute scaled error*
$$\mathrm{MASE} = \frac{\sum{i}|y_{i} - f_{i}|}{\sum_{i} |y_{i} - \bar{y}|}$$

where $$\bar{y} = \frac{\sum_{i} y_{i}}{N}$$ is the mean of the true values

is similar to the weighted mean absolute percentage error, but scaled by the sum of the absolute deviations rather than the sum of the absolute values. It gives equal weight to positive and negative errors.

The *mean absolute log error*

$$\mathrm{MALE} = \frac{\sum_{i}|\ln y_{i} - ln f_{i}|}{N}$$
gives equal weight to positive and negative errors, but requires the forecasted and actual values to be strictly positive, or it will diverge.

Another important metric is the *coefficient of determination*, or *explained variance*

$$R^{2} = 1 - \frac{\sum_{i}(y_{i} - f_{i})^{2}}{\sum_{i} (y_{i} - \bar{y})^2}$$

This can be seen as bearing a similar relationship to the mean squared error as the mean absolute scaled error has to the mean absolute error. It is a measure of how successful a model is at predicting the variability of the data. It is less sensitive to outliers that MSE, because an outlier will increase the denominator as well as the numerator. It is equivalent to the square of [Pearson correlation coefficient]({% link Key-Algorithms/metrics.md %}) between the actual and predicted values.

All these metrics test primarily for random errors. If we wish to test for systematic errors we can use the *mean signed difference*

$$\mathrm{MSD} = \frac{\sum_{i} y_{i} - f_{i}}{N}$$ 

which indicates the magnitude and direction of any likely bias in the model.

by [Dr Peter J Bleackley]({% link index.md %})

[Key Algorithms]({% link Key-Algorithms/index.md %})

Model Evaluation
: [Evaluation Metrics for Classifiers]({% link Key-Algorithms/evaluation-metrics-classifiers.md %})
: *Evaluation Metrics for Regression*
: [Cross Validation]({% link Key-Algorithms/cross-validation.md %})

