---
title: Data Science Notebooks - The Entropy of "Alice in Wonderland"
---

# The Entropy of Alice in Wonderland

Several years ago. I read in [New Scientist](https://www.newscientist.com/) about an information theory based technique for identifying the most significant words in a document, according to the role they play in its structure. After looking up the paper, [Towards the quantification of semantic information in written language](https://arxiv.org/abs/0907.1558) by Marcello Montemurro and Damian Zanette, I implemented the algorithm and contributed it to [Gensim](https://radimrehurek.com/gensim/). Unfortunately, it's no longer in the latest release, but I have created a [fork of Gensim](https://github.com/PeteBleackley/gensim) to allow further development of features that have been dropped from the latest release.

When I found the text of *Alice's Adventures in Wonderland* as a Kaggle Dataset, it provided the opportunity to create a demonstration for the algorithm.

<iframe frameborder="0" height="800" scrolling="auto" src="https://www.kaggle.com/embed/petebleackley/entropy-based-keyword-extraction?kernelSessionId=34819997" title="Entropy Based Keyword Extraction" width="100%"></iframe>

I also created a video explaining it.

<iframe width="560" height="315" src="https://www.youtube.com/embed/zC4ZXvAxnHA" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

by [Dr Peter J Bleackley]({% link index.md %})

[Data Science Notebooks]({% link Data-Science-Notebooks/index.md %})
