---
layout: default
title: Emily
---

I have started an Open Source information retrieval system on CodeBerg, which I call [Emily](https://https://codeberg.org/PeteBleackley/Emily), [because it finds things](http://www.smallfilms.co.uk/bagpuss/index.htm).

Emily consists of the usual components - embedding model, vector index, sparse indices and reranker. The embedding model and reranker can be configured, but I chose as defaults small models that perform well on the [MTEB Benchmarks](https://huggingface.co/spaces/mteb/leaderboard).


For the sparse indices, I have used the well-known [Okapi BM25 algorithm](https://en.wikipedia.org/wiki/Okapi_BM25). However, for long documents this has the disadvantage that a number of search terms may be individually significant, but not related to each other. Suppose you have an archive of recipe books, and you're searching for "Lamb and apricot pie". One recipe book has lots of recipes for lamb dishes, lots of recipes that include apricots, and lots of pies, but not the specific recipe you're looking for. I've therefore devised a [Cooccurrence Index]({% link Emily/CooccurrenceIndex.md %}), which allocates greater significance to documents where the search terms occur in the same sentences.

To ensure that Emily will perform well at scale, I have implemented the sparse indices with [Polars](https://docs.pola.rs/), which allows for lazy evaluation and distributed computation. 

Emily is released under the [MIT Licence](https://codeberg.org/PeteBleackley/Emily/src/branch/main/LICENCE.md), and its [API documentation](https://emily.readthedocs.io) can be found on ReadTheDocs. I am looking for help to develop the system further, especially with regard to testing. 

by [Dr Peter J Bleackley]({% link index.md %})
