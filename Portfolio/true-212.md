---
layout: default
title: Case Studies -True 212
---

# True 212

## The Client

[True 212](https://www.true212.com/)

## The Problem

True 212 wanted to identify relevant content to link to from their news and culture blogs. They believed that a simple bag-of-words approach would lead to naive matches, and wished to extract semantics from the documents to enable richer matches.

## The Approach

A NLP pipeline was created with the following stages.

A Named Entity Recognition system that identified candidate named entities in a document and found corresponding [WikiData](https://www.wikidata.org/) entities. Known relationships between WikData entities were used to disambiguate candidate matches.

A Part of Speech Tagger that used [Hidden Markov Models]({% link Key-Algorithms/hidden-markov-models.md %}) to return a the probability distribution over the part of speech categories used in WordNet for each word in a sentence.

A Word Sense Disambiguation component that used the [Viterbi algorithm]({% link Key-Algorithms/viterbi-algorithm.md %}) to find the maximum likelihood sequence of [WordNet](https://wordnet.princeton.edu/) IDs corresponding to the words in a given sentence, allowing for stopwords, multiword expressions, named entities and out-of-vocabulary words. This achieved state-of-the-art accuracy (70%).

A Latent Semantic Indexing model which was trained on the semantically enhanced documents to perform rich matching.

## Technology Used

* [Numpy](https://numpy.org/)
* [Scipy](https://www.scipy.org/)
* [Scikit-Learn](https://scikit-learn.org/stable/)
* [Pandas](https://pandas.pydata.org/)
* [Gensim](https://radimrehurek.com/gensim/index.html)
* [MongoDB](https://www.mongodb.com/)

by [Dr Peter Bleackley]({% link index.md %})

[Case Studies]({% link Portfolio/index.md %})

