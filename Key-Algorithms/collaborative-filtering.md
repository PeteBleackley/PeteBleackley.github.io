title: Key Algorithms - Collaborative Fitering

#Collaborative Filtering

A problem of interest to a lot of businesses is *recommedation* - how to predict what their customers are likely to want. One of the simplest approaches to this is *Collaborative Fitering*, which works by identifying users with similar tastes.

Suppose each user $i$ has rated a set of items $R_{i}$, giving each item $n$ a score $S_{i,n}$. For a second user $j$, we can obtain the intersection of their rated items $R_{i} \cap R_{j}$ and from these compute a weight $w_{ij}$ using a suitable [similarity metric]({% link metrics.md %}) on the scores each user has given to their common items. The most common metrics to use would be the cosine similarity or Pearson correlation. If all scores are positive or zero, cosine similarity will give weights in the range $0 \le w_{ij} \le 1$, whereas the Pearson correlation will give weights in the range $-1 \le w_{ij} \le 1$, which is potentially more sensitive to polarisation in people's tastes. If two users have no items in common, $w_{ij} = 0$.

For an item $n$ which user $i$ has not rated, we may then calculate a predicted rating
$$S^{\prime}_{i,n} = \frac{\sum_{j \mid n \in R_{j}} S_{j,n} w_{ij}}{\sum_{j \mid n \in R_{j}} w_{ij}}$$

This is the weighted mean of other user's weightings for the item, weighted according to the similarity of the users' ratings on other items. Items with a high predicted score for a given user can then be recommended to that user. The name *Collaborative Filtering* refers to the users collaborating through the algorithm to filter the items according to each other's preferences.

So far we have assumed that users have rated the items with a numerical score. However, in many applications, we only have a binary choice - for example, whether users have purchased an item, or shared a link. In this case, we can use the Tanimoto metric
$$w_{ij} = \frac{|R_{i} \cap R_{j}|}{|R{i} \cup R_{j}|}$$ as the weighting between users. The Predicted rating for $n$ then becomes
$$S_{i,n} = \frac{\sum_{j \mid n \in R_{j}} w_{ij}}{|\left\{j \mid n \in R_{j}\right\}|}$$
that is, the average similarity to the user of users who have chosen the item.

Collaborative filtering and other recommendation algorithms suffer from the *bootstrap problem*, in that they require a lot of user data to work effectively, but when starting something up, that data is not available. Until a user has rated a significant number of items, it will not be possible to predict accurately what they will like, and until a significant number of people have rated an item, it will not be possible to predict accurately who will like it. As a result, recommendation systems cannot function effectively as a product in their own right, but work best as a feature of a larger product.

by [Dr Peter J Bleackley]({% link index.md %})

[Key Algorithms]({% link Key-Algorithms/index.md %})

Collective Intelligence
: *Collaborative Filtering*
: [PageRank]({% link Key-Algorithms/pagerank.md %})

