title: Key Algorithms - Similarity and Distance Metrics

#Similarity and Difference Metrics 

Data scientists often need to compare data points. This is necessary for indexing data, for finding clusters in datasets, for detecting outliers and anomalies, for comparing user behaviour in recommendations systems, and for measuring quality of fit when predicting continuous variables. There are various metrics that can be used for this purpose.

One of the most frequently used metrics is *Euclidean distance*. For two vectors $\vec{x}$ and $\vec{y}$, this is given by 
$$S = |\vec{x} - \vec{y}| \\
= \sqrt{\sum_{i} (x_{i} - y_{i})^2}$$
This is analogous to distances in physical space. It is useful when the overall scale of the data is important, and has the property that *smaller is better*.

When we wish to take the overall scale of the data out of consideration, it is common to use *cosine similarity* $$C = \frac{\vec{x} \cdot \vec{y}}{|\vec{x}||\vec{y}|}$$
This represents the cosine of the angle between the two vectors, measured from the origin. It has a range of -1 to +1 and *bigger is better*. (If all the components of the vectors are positive, the range is from 0 to 1.) A variation on this is the *Pearson correlation*
$$P = \frac{(\vec{x} - \bar{x}) \cdot (\vec{y} - \bar{y})}{|\vec{x} - \bar{x}||\vec{y} - \bar{y}|}$$ where $\bar{x}$ and $\bar{y}$ are the means of the components of $\vec{x}$ and $\vec{y}$ repsectively. This measures the degree to which the components of the two vectors are linearly correlated with each other.

These metrics are all most useful when the ranges of all the components are similar. Otherwise, the effects of the components with the largest ranges will tend to dominate over those with smaller ranges. The usual remedy for this is to scale the components as $$\vec{x^{\prime}} = \frac{\vec{x} - \bar{\vec{x}}}{\vec{\sigma}}$$ where $\bar{\vec{x}}$ and $\vec{\sigma}$ are the mean and covariance of the sample respectively. Another possibility is to use the *Mahalanobis distance*
$$M = \sqrt{(\vec{x} - \vec{y}) \cdot \mathbf{\Sigma}^{-1} \cdot (\vec{x} -\vec{y})}$$
where $\mathbf{\Sigma}$ is the *covariance matrix*
$$\mathbf{\Sigma} = \frac{\sum_{i}(\vec{x_{i}} - \bar{\vec{x}}) \otimes (\vec{x_{i}} - \bar{\vec{x}})}{N}$$ where $N$ is the number of samples. This not only scales the variables appropriately, but accounts for dependencies between them. It is, however, more computationally expensive, especially for high-dimensional data.

Sometimes we wish to compare data that is not readily described as vectors. Suppose that we wish to compare two users of a social network in terms of which links they have shared. We might consider the links shared by each user as a set of unique items. To compare these sets, we can use the *Tanimoto metric*
$$T = \frac{|A \cap B|}{|A \cup B|}$$, that is the fraction of the links shared by either user that have been shared by both users. This has a range from 0 to 1 and *bigger is better*.

If we wish to compare two short strings (as for example, in a spellchecking application), the ususal method is the *Leveshtein distance* . This is the number of insertions, deletions or substitutiions needed to transform one string into another. If we consider the strings $X$ and $Y$ as sequences of characters $x_{1}x_{2}\ldots x_{m}$ and $y_{1}y_{2}\ldots y_{n}$ respectively, we can define an $m \times n$ matrix $\mathbf{L}$ as 
$$L_{i,0} = i$$ for $i$ from 0 to m
$$L_{0,j} = j$$ for $j$ from 0 to n
$$L_{i,j} = \min \left(L_{i,j-1},L{i-1,j},L{i-1,j-1}+\left\{\begin{array}{c 1} 0 & \quad \mathrm{if } x_{i} = y_{j} \\
1 & \quad \mathrm{if } x_{i} \neq y_{j} \end{array} \right.\right)$$

The Levenshtein distance is then $L_{m,n}$. While simple to implement and intuitive to understand, this is only really suitable for comparing short strings, as the complexity is $\mathcal{O}(m \times n)$.

A wide variety of distance metrics are implemented in [Scipy](https://docs.scipy.org/doc/scipy/reference/spatial.distance.html)

By [Dr Peter J Bleackley]({% link index.md %})
 
 [Key Algorithms]({% link Key-Algorithms/index.md %})
 
 Measurements
: *Similarity and Difference Metrics*
: [TF-IDF]({% link tf-idf.md %})
: [Data Reduction]({% link data-reduction.md %})
: [Latent Semantic Indexing]({%link latent-semantic-indexing.md %})
: [Information Theory]({% link information-theory.md %})

