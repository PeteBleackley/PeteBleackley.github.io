title: Key Algorithms - Cross Validation

# Cross Validation

When training a model, standard practice is to hold back part of the dataset for testing. This ensures that we have tested the model's ability to generalise to unseen data.

However, many models have *hyperparameters*, such as the regularisation penalties used in [regularised linear models]({% link Key-Algorithms/linear-regression.md %}). In order to select the best values for these hyperparameters, it is necessary to try fitting the model with different values of hyperparameters and select the version that gives the best results. However, if we use the same test dataset for hyperparameter selection as we do for overall model testing, there is a risk that the hyperparameters will themselves be overfit to the test dataset.

One solution to this is to further subdivide the dataset into training, validation and test datasets. We use the valuation dataset to assess which hyperparameters give the best performance, and then use the test dataset to evaluate how well this model performs on unseen data. Many publicly available datasets come partitioned in this way. However, if we have a limited amount of data to work with, we may find that this approach reduces the training dataset too much.

An alternative to this is *Cross Validation*. The basic procedure is to make several different partitions of the data into training and validation sets, and calculates the average of the evaluation metrics across the different partitions. This, while more computationally expensive than using a single validation partition, gives more robust results, since the choice of hyperparameters will not depend on the results from a single validation partition. Once hyperparemeters have been chosen, the data used for validation can then be folded back into the training dataset to train the final model.

Several strategies may be used for making the split. The simplest is the *Leave One Out* strategy. For a training dataset of size $N$, this makes $N$ partitions into $N-1$ training examples and 1 validation example. A variation of this is *Leave P Out*, which makes $\binom{N}{P}$ partitions of $N-P$ training examples and $P$ validation examples. These methods are computationally espensive have the disadvantage that there is considerable overlap between the partitions, so their results are not independent.

A more commonly used strategy is *K-Fold Cross Validation*. This divides the data into $K$ *folds* of $\frac{N}{K}$ examples. Each of these in turn is used as the validation partition, with the remaining folds combined to form the training partition. Usually 5 or 10 folds are used. This is more efficient than Leave One Out, and provides greater independence between tests, as each training dataset overlaps by only $\frac{K-2}{K-1}$ with the others, as opposed to almost complete overlap in Leave One Out. For further statistical rigour (at the expense of greater compute time) *Repeated K-Fold Cross Validation* performs this several times, with different assignments of examples to folds. 

If the classes to be predicted are highly unbalanced, there is a risk that some folds may not contain any examples of a particular class, thus skewing the results. *Stratified K-Fold Cross Validation* addresses this problem by grouping the examples by target class, and then dividing each class equally between the folds. If there are know statistical dependencies in the training examples, *Group K Fold* divides the dataset into groups according to some feature which is expected to have impotant staticstical correlations with other variables, and assigns the data to folds group by group, so that the same group is never present in both the training and validation dataset. This ensures that the model will generalise across groups. Group K-Fold relaxes the requirement that folds be of equal size. These two strategies can be combined as *Stratified Group K-Fold Cross Validation*.

Related to Group K-Fold is the *Leave One Group Out* strategy, which in effect treats each group as a fold, and the *Leave P Groups Out*, strategy, which, given $G$ groups, forms $\binom{G}{P}$ partitions, each containing $G-P$ groups in the training dataset and $P$ groups in the test dataset.

Another possible strategy for *Shuffle Split Cross Validation*. In this, the dataset is repeatedly shuffled, and after each shuffle split into a training and validation dataset. Whereas with K-Fold cross validation and its variants the size of the validation dataset is dependent on the number of iterations, in Shuffle Split Cross Validation they may be selected independently of each other. Stratification and Grouping may be applied to Shuffle Split as they are to K-Fold.

In my work at [Pentland Brands]({% link Portfolio/pentland-brands.md %}) I had to evaluate a large number of candidate models. K-Fold Cross Validation played an essential role in this.

by [Dr Peter J Bleackley]({% link index.md %})

[Key Algorithms]({% link Key-Algorithms/index.md %})

Model Evaluation
: [Evaluation Metrics for Classifiers]({% link Key-Algorithms/evaluation-metrics-classifiers.md %})
: [Evaluation Metrics for Regression]({% link Key-Algorithms/evaluation-metrics-regression.md %})
: *Cross Validation*

