title: Key Algorithms - TF-IDF

#TF-IDF

In the post on [Similarity and Distance Metrics]({% link Key-Algorithms/metrics.md %}), we mentioned that Levenshtein distance is only suitable for comparing short strings. One reason for this, as previously discussed, is computational complexity, but another is that by comparing *characters*, it says nothing about the *meaning* of what it compares.

So, what can we do if we want to compare large documents in a meaningful way? One thing we could do is compare word frequencies. Of course, we need to take the overall length of the document into account, so we define the *Term Frequency*

$$\mathrm{TF}_{w} = \frac{n_{w}}{\sum_{i} n_{i}}$$ where $n_{w}$ is the number of times word $w$ occurs in the document. Using this, we could compute a Euclidean distance or cosine similarity between two documents. 

However, not all words are equally important. If we are talking about *an algorithm*, we can easily see that the content word *algorithm* is more important that the function word *an*. Given a corpus of $D$ documents, of which $D_{w}$ contain word $w$, we then define the *Inverse Document Frequency*

$$\mathrm{IDF}_{w} = \log \frac{D}{D_{w}+1}$$

Adding 1 to the denominator ensures we never divide by zero. You may wonder why we have to do this, since there will be no words in the corpus that do not occur in any documents. However, if we are continually adding documents to our corpus, it would be a major expense to have to recalculate all the previous documents when one was added that contained new vocabulary. To avoid that, we might want to use a fixed dictionary that is provided in advance. However, if our corpus is fixed, and we know that all words will occur in at least one document, we can use $D_{w}$ as the denominator.

This measures the ability of a word to discriminate between documents in the corpus. For a document $d$ and a word $w$ we can then combine these two measures to define *TF-IDF* as

$$\mathrm{TFIDF}_{w,d} = \mathrm{TF}_{w,d} \mathrm{IDF}_{w} = \frac{n_{w,d}}{\sum_{i} n_{i,d}} \log \frac{D}{D_{w}+1}$$
which measures the importance of the word in the document weighted by its importance in the corpus. A word that occurs frequently in a few documents but is absent in many will be important for identifying those documents.

One way we can use TF-IDF is to search a corpus of documents. Given a query $Q = w_{1}w_{2}\ldots w_{n}$ we can calculate a score for a document $d$

$$S_{d} = \sum_{i} \mathrm{TFIDF}_{w_{i},d}$$ and retrieve the documents with the highest scores.

TF-IDF is an example of a *bag of words* model - one based entirely on word frequencies that takes no account of grammar or context. An implementation (to which I have contributed a bug fix) can be found in the [Gensim](https://radimrehurek.com/gensim/) topic modelling library.

By [Dr Peter J Bleackley]({% link index.md %})
 
 [Key Algorithms]({% link Key-Algorithms/index.md %})
 
 Measurements
: [Similarity and Difference Metrics]({% link Key-Algorithms/metrics.md %})
: *TF-IDF*
: [Data Reduction]({% link Key-Algorithms/data-reduction.md %})
: [Latent Semantic Indexing]({% link Key-Algorithms/latent-semantic-indexing.md %})
: [Information Theory]({% link Key-Algorithms/information-theory.md %})

