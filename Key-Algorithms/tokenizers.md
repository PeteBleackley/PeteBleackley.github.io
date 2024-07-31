title: Key Algorithms - Tokenizers

#Tokenizers

As mentioned in the article on [transformers]({% link Key-Algorithms/transformers.md %}), most NLP models start by dividing the text into *tokens*. The simplest way of doing this is simply to split the text on whitespace or non-alphabetic characters. [Gensim's tokenize function](https://radimrehurek.com/gensim/utils.html#gensim.utils.tokenize) does this, and it is adequate for bag-of-words models like [TF-IDF]({% link Key-Algorithms/tf-idf.md %}) or [Latent Semantic Indexing]({% link Key-Algorithms/latent-semantic-indexing.md %}). However, it has a number of disadvanteges.

1. Not all languages divide words on spaces - Mandarin and Japanese do not.
2. Words related to each other by prefixes and suffixes are treated as entirely different words - for example, "surprise", "surprised", "surprising", "surprisingly"  and "unsurprisingly" would not be treated as related words. This is particularly a problem for languages whose morphology carries more information than English's does. Stemming or lemmatisation can partially address this, but discards the information in the affixes.
3. Assigning numerical indexes to rare words becomes a problem. A rare word that is seen during training will get an index, but may not be encountered in subsequent use, whereas a word that has not been seen during training cannot be assigned indexes in subsequent use.

While some models, such as those based on [Recurrent Neural Networks]({% link Key-Algorithms/recurrent-neural-networks.md %}) simply use characters as input, this of course means that the meaning of words must be inferred from scratch at every invocation of the model. 

*Subword tokenizers* attemrpt to identify meaningful sequences within the input, which may correspond to words, word stems, affixes or punctuation. This we would expect "unsurprisingly" to be tokenized as "un-surpris-ing-ly". This is useful for transformer models, where the [Attention Mechnanism]({% link Key-Algorithms/attention.md %}) can then use each subword to modify the meaning of the others.

The simplest method for this is *Byte Pair Encoding*. This starts by treating each byte of the training corpus as a token. It then calculates the frequencies of each pair of tokens occurring in the corpus, and replaces the most frequenct pair with a new token representing that pair, and learns a *merge rule* for that pair of tokens This process repeats until a vocabulary of a given size has been obtained. To tokenize a text, it applies the merge rules to the text.

A variation of this is the *WordPiece* tokenizer. Rather than picking tokens to merge by their overall frequency, this uses a score 
$$S_{ij} = \frac{F_{ij}}{F_{i} F_{j}}$$ where $F_{i}$, $F_{j}$ and $F_{ij}$ are the frequencies of tokens $i$, $j$ and the sequence $ij$ respectively. Rather than using merge rules, it tokenises texts by finding the longest substring at the beginning of the text that is in its vocabulary, splitting it off as a token, and repeating until the entire text has been divided into tokens.

These methods build their vocabulary from the bottom up. In contrast, the *Unigram* tokenizer builds its vocabulary from the top down. It starts by calculating the frequencies of all the words and their substrings in the training corpus, and discards infrequent tokens until the required vocabulary size is reached. To tokenize a text, it uses the [Viterbi algorithm]({% link Key-Algorithms/viterbi-algorithm.md %}) to find the maximum likelihood sequence of tokens corresponding to the text.

*SentencePiece* is an algorithm that aims to account for multiple possible segmentations in a more robust way than either byte pair encoders or unigram models. It is trained by the following procedure.

1. Train a byte pair encoder or unigram tokenizer with a larger vocabulary size than we ultimately need.
2. Use the Viterbi algorithm to calculate the maximum likelihood segmentation of the training corpus according to this model.
3. Recalculate the token frequencies according to the maximum-likelihood segmentation.
4. Repeat steps 2 and 3 until the token frequencies converge
5. Prune the model down to the required vocabulary size.

To tokenise a text, SentencePiece maximises the score
$$S = \frac{\log P(\mathbf{y} \mid \mathbf{x})}{|\mathbf{y}|^{\lambda}}$$
Where $\mathbf{y}$ is the tokenized text, $\mathbf{x}$ is the untokenized text, $|\mathbf{y}|$ is the length of the tokenized text, and $\lambda$ is a constant. 


The [HuggingFace Tokenizers Library](https://huggingface.co/docs/tokenizers/index) contains implementations of byte pair encoding, WordPiece and unigram tokenizers, and pretrained tokenizers are available for models obtained from HuggingFace, and [SentencePiece](https://pypi.org/project/sentencepiece/) can be found on PyPI.

By [Dr Peter J Bleackley]({% link index.md %})
 
 [Key Algorithms]({% link Key-Algorithms/index.md %})
 
 Components of Neural Networks
 : [The Chain Rule and Backpropogation]({% link Key-Algorithmschain-rule.md %})
 : [Activation Functions]({% link Key-Algorithms/activation-functions %})
 : [Loss Functions]({% link Key-Algorithms/loss-functions.md %})
 : [Gradient Descent]({% link Key-Algorithms/gradient-descent.md %})
 : [Transfer Learning]({% link Key-Algorithms/transfer-learning.md %})
 : *Tokenizers*
