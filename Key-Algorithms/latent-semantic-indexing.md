title: Key Algorithms -Latent Semantic Indexing
#Latent Semantic Indexing

In the article on [data reduction]({% link Key-Algorithms/data-reduction.md %}), we mentioned the *curse of dimensionality*, whereby large numbers of features make data increasingly difficult to analyse meaningfully. If we take another look at [TF-IDF]({$ link Key-Algorithms/tf-idf.md %}), we see that this will generate a feature for each unique word in the corpus that it is trained on, which may be in the tens of thousands. It therefore makes sense to apply a data reduction method and obtain a more compact representation.

TF-IDF, as previously discussed, makes use of the fact that words that occur in some documents but not others are the most useful for distinguishing between the documents. This means that its feature vectors will generally be quite sparse. Therefore, the most appropriate data reduction method to use will be Singular Value Decomposition.

$$\mathbf{TFIDF} \approx \mathbf{U} \cdot \mathbf{\Sigma} \cdot \mathbf{V}^{T}$$

Tyypically around 200 components are retained. The left singular vectors $\mathbf{U}$ then represent documents in the lower-dimensional space, while the right sigular vectors $\mathbf{V}$ represent words in the same space. Words that tend to appear in the same documents will tend to have similar vector representations, and according to the *distributional hypothesis*, this gives an implicit representation of their meaninng. This implicit representation of meaning gives the technique the name *Latent Semantic Analysis*.

Given a query $Q = w_{1}w_{2}\ldots w_{n}$, we can calculate a query vector
$$\vec{q} = \sum_{i}\mathbf{V}_{w_{i}}$$
We can then search our corpus for the most relevant documents to match the query by calculating a score
$$S = \mathbf{U} \cdot \vec{q}$$ and selecting the documents with the greatest score. Since it can be used to search the corpus in this way, Latent Semantic Analysis is also known as *Latent Semantic Indexing*.

An implementation of Latent Semanic Indexing (LSI) can be found in the [Gensim](https://radimrehurek.com/gensim/models/lsimodel.html) library, along with several other *topic models*, which similarly attempt to use the distributional hypothesis to characterise documents.

While LSI can account for different words having similar meanings, it is still a bag of words model and cannot account for the same word having different meanings dependent on context. In my work at [True 212]({% link Portfolio/true-212.md %}) I attempted to address this issue by building an NLP pipeline that enriched the documents with Named Entity Recognition and Word Sense Disambiguation before applying LSI, but modern transformer models address it by calculating contextual word vectors. It can, however, be seen as the distant ancestor of these models.

By [Dr Peter J Bleackley]({% link index.md %})
 
 [Key Algorithms]({% link Key-Algorithms/index.md %})
 
 Measurements
: [Similarity and Difference Metrics]({% link Key-Algorithms/metrics.md %})
: [TF-IDF]({% link Key-Algorithms/tf-idf.md %})
: [Data Reduction]({% link Key-Algorithms/data-reduction.md %})
: *Latent Semantic Indexing*
: [Information Theory]({% link Key-Algorithms/information-theory.md %})

