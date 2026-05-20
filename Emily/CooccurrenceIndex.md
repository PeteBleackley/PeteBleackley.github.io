---
layout: default
title: Emily: Cooccurrence Index
---
{% include maths.html %}

The Cooccurrence Index component of Emily is intended to detect documents where the relationship between words in the query is significant. To do this, it uses ideas from [Information Theory]({% link Key-Algorithms/information-theory.md %}).

Consider two words, $a$ and $b$, that appear with probability $P\_{a}$ and $P\_{b}$ respectively in sentences randomly chosen from a document $D$. If two words are unrelated to each other in the context of the document, the probability of them occurring together in the same sentence will be given by 

$$P_{ab} = P_{a} P_{b}$$

We can therefore calculate the degree to which they are correlated as

$$M_{ab} = \log_{2} \frac{P_{ab}}{P_{a} P_{b}}$$

Given that the document contains $N$ sentences, of which $n\_{a}$ contain word $a$, $n\_{b}$ contain word $b$, and $n\_{ab}$ contain both, this becomes

$$M_{ab} = \log_{2} \frac{\frac{n_{ab}}{N}}{\frac{n_{a}}{N} \frac{n_{b}}{N}} \\
 = \log_{2} \frac{n_{ab} N}{n_{a} n_{b}} \\
 = \log_{2} n_{ab} + \log_{2} N - \log_{2} n_{a} - \log_{2} n_{b}$$
 
 Theoretically this may be negative if the distribution of the words in anticorrelated.
 
 When indexing documents, each sentence within the document is represented by a sorted list of unique words, with stopwords excluded. A query $q$ is represented by a similar list,
 $q = w\_{0},w\_{1} \\dots w\_{i} \\dots \ w\_{k}$
 
 We may then calculate a score for a document $D$ given query $q$ as
 
 $$S_{D \mid q} = \sum_{i=0}^{k-1} \sum_{j=i+1}^{k} M_{w_{i}w_{j} \mid D} \\
 = \sum_{i=0}^{k-1} \sum_{j=i+1}^{k} \log_{2} n_{w_{i} w_{j} \mid D} + \log_{2} N_{D} - \log_{2} n_{w_{i} \mid D} - \log_{2} n_{w_{j} \mid D} $$
 
For any pairs of words from the query that do not occur in the document, we impose $M\_{w\_{i} w\_{j} \mid D} = 0$
 
After calculating the score, any documents with a negative score are excluded, and the rest are filtered according to the Pareto principal, so as to return the greatest fraction of the total score with the smallest fraction of the candidate documents. For more information about this, see [How Many Components]({% link how_many_components.md %}).

The source code for this can be seen on the [CodeBerg Emily repository](https://codeberg.org/PeteBleackley/Emily/src/branch/main/emily/index_classes/CooccurrenceIndex.py)

[Emily]({% link Emily/index.md %})
by [Dr Peter J Bleackley]({% link index.md %})


