---
title: Code Reviewing ChatGPT
---

{% include maths.html %}

# Code Reviewing ChatGPT 
Since the introduction of ChatGPT, there has been a lot of hype about the abilities of Large Language Models. The buzzword of the moment is *vibe coding*, and there was a lot of schadenfreud on the web when a man who tried to build a SAAS empire with AI-generated code found that it all fell apart after two weeks. However, **we want facts!** I decided to set ChatGPT the task of coding a classical NLP problem that I understood, and see how well it did. In previous projects, I've used the [Viterbi algorithm]({% link Key-Algorithms/viterbi-algorithm.md %}) to perform Word Sense Disambiguation, so I gave it the following prompt

> Write a Python class that uses the Viterbi algorithm to perform word sense disambiguation. It's `__call__` method should take a string representing a sentence and output the 
> maximum likelihood sequence of WordNet ids corresponding to that sentence. Take into account multi-word expressions, proper nouns, function words and out-of-vocabulary words. The 
> component should be trained on the Semcor corpus. You may use any well-known Python library. Ensure all methods have good docstrings.

Its first attempt didn't take multi-word expressions into account, but after further prompting it gave me the following.

```python
import nltk
from nltk.corpus import semcor, wordnet as wn
from nltk.util import ngrams
from collections import defaultdict
import numpy as np

class ViterbiWSD:
    """
    A class for performing Word Sense Disambiguation (WSD) using the Viterbi algorithm.
    Trained on the Semcor corpus, it assigns the most likely WordNet senses to words in a sentence.
    """
    
    def __init__(self):
        """
        Initializes the WSD model by training on the Semcor corpus.
        """
        self.transition_probs = defaultdict(lambda: defaultdict(float))
        self.emission_probs = defaultdict(lambda: defaultdict(float))
        self.states = set()
        self.multiword_expressions = set()
        self.train()
    
    def train(self):
        """
        Train the model using the Semcor corpus, including multi-word expressions.
        """
        bigram_counts = defaultdict(lambda: defaultdict(int))
        unigram_counts = defaultdict(int)
        emission_counts = defaultdict(lambda: defaultdict(int))
        
        for sentence in semcor.tagged_sents(tag='both'):  # Extract senses and words
            prev_sense = None
            tokens = []
            for word in sentence:
                if isinstance(word, nltk.tree.Tree):
                    senses = word.label()  # WordNet synsets
                    if isinstance(senses, list):
                        sense = senses[0]  # Take the first sense as default
                        token = " ".join(w.lower() for w in word.leaves())
                        tokens.append(token)
                        if " " in token:
                            self.multiword_expressions.add(token)
                        self.states.add(sense)
                        emission_counts[sense][token] += 1
                        unigram_counts[sense] += 1
                        if prev_sense:
                            bigram_counts[prev_sense][sense] += 1
                        prev_sense = sense
        
        # Compute probabilities
        for s1 in bigram_counts:
            total = sum(bigram_counts[s1].values())
            for s2 in bigram_counts[s1]:
                self.transition_probs[s1][s2] = bigram_counts[s1][s2] / total
        
        for sense in emission_counts:
            total = sum(emission_counts[sense].values())
            for token in emission_counts[sense]:
                self.emission_probs[sense][token] = emission_counts[sense][token] / total
    
    def detect_mwes(self, words):
        """
        Detects and merges multi-word expressions in a list of words.
        :param words: A list of tokenized words.
        :return: A list of words with MWEs merged.
        """
        merged = []
        i = 0
        while i < len(words):
            found_mwe = False
            for n in range(3, 0, -1):  # Check trigrams, bigrams, then unigrams
                if i + n <= len(words):
                    candidate = " ".join(words[i:i+n])
                    if candidate in self.multiword_expressions:
                        merged.append(candidate)
                        i += n
                        found_mwe = True
                        break
            if not found_mwe:
                merged.append(words[i])
                i += 1
        return merged
    
    def __call__(self, sentence):
        """
        Perform WSD on an input sentence, handling multi-word expressions.
        :param sentence: A string representing a sentence.
        :return: A list of WordNet sense IDs corresponding to the most likely sequence.
        """
        words = nltk.word_tokenize(sentence.lower())
        words = self.detect_mwes(words)
        n = len(words)
        dp = [{} for _ in range(n)]
        backpointer = [{} for _ in range(n)]
        
        # Initialization step
        for state in self.states:
            dp[0][state] = self.emission_probs[state].get(words[0], 1e-6)
            backpointer[0][state] = None
        
        # Viterbi recursion
        for t in range(1, n):
            for state in self.states:
                max_prob, best_prev_state = max(
                    ((dp[t-1][prev_state] * self.transition_probs[prev_state].get(state, 1e-6) *
                      self.emission_probs[state].get(words[t], 1e-6), prev_state)
                     for prev_state in self.states),
                    key=lambda x: x[0]
                )
                dp[t][state] = max_prob
                backpointer[t][state] = best_prev_state
        
        # Backtrace to find the best path
        best_last_state = max(dp[n-1], key=dp[n-1].get)
        best_path = [best_last_state]
        for t in range(n-1, 0, -1):
            best_path.insert(0, backpointer[t][best_path[0]])
        
        return [s.name() for s in best_path]
```

So how did it do? Looking at the `import` statements at the beginning of the file, we notice `from nltk.utils import ngrams`, but this isn't used anywhere in the code. In the `train` method, we find function words (*ie* tokens that do not have a corresponding WordNet id) are simply discarded. The calculation of the emission and transition probabilities is mostly correct, but assumes the label of each chunk is a `list`. Since it is actually a `Lemma` object, no training will actually take place. The WordNet id we actually want is not the lemma, but the synset, which would be accessed by calling `word.label().synset()`. There is also a variable called `unigram_counts` which is calculated during training but never used. 

Let's assume we fix these errors and look at the rest of the code.

The detection of multi-word expressions is quite simple. For each word in a sentence, it examines a window whose with is initially 3 tokens, and decreases to 1 token, and if the contents of that window match a known multi-word expression, it replaces the tokens in that window with that expression. This makes several assumptions
1. That no multi-word expression is longer than 3 tokens
2. That no multi-word expression is a substring of another multi-word expression.
3. That if two overlapping sequences within a sentence are potential multi-word expressions, the one that begins first will be the correct one.
4. That a sequence of words that can be a multi-word expression always represents that expression.

None of these assumptions is necessarily correct. In fact, the first multi-word expression found in Semcor is *Fulton County Grand Jury*. We can therefore expect incorrect detection of multi-word expressions to occur. The `detect_mwes` method also checks for multi-word expressions of length 1.

Now let us consider the `__call__` method, where inference takes place. While the `train` method ignores tokens that are not labelled with WordNet ids, the `__call__` method has no way of doing this. It will therefore try to retrieve WordNet ids corresponding to words that should not have them. Inference will be inconsistent with training.

Related to this, the model creates a hidden state for every WordNet id seen during training for each word in the sentence, setting a default probability of $10^{-6}$ for any state that does not correspond to that word. On top of this, no pruning of the trellis is carried out. This means that the memory requirements for a sentence of length $L$ will be $\mathcal{O} (L \times N)$, where $N$ is the number of WordNet ids represented in Semcor. Since each state generated will have to search all the states at for the previous token to find its maximum likelihood predecessor, the time complexity will be $\mathcal{O} (L \times N^{2})$.

On top of this, the probabilities at each timestep are never normalised. For a sentence of any significant length, the probabilities will become very small and may underflow. When this happens, a state whose probability has underflowed cannot be chosen as the predecessor of any subsequent state, even if it would have been the best choice if the probabilities had been normalised. In the worst case scenario, all the probabilities may underflow. Choice of predecessors would then be completely arbitrary.

When calculating the maximum likelihood path at the end, the code starts by finding the maximum likelihood state at `dp[n-1]`. While this isn't exactly wrong, Python's indexing syntax would make it simpler to put `dp[-1]`. Iterating backwards over the trellis, it builds its output by repeatedly calling `best_path.insert(0, backpointer[t][best_path[0]])`. This is probably the most inefficient way to reverse a list in Python. Also, if the trellis had been pruned as is normal in the Viterbi algorithm, then after choosing the best final state and pruning, there would be only one state left at each stage of the trellis, and so no reversal would be necessary.

So, ChatGPT has given us a very inefficient and inaccurate implementation of Word Sense Disambiguation, and hasn't even followed its brief that well. Based on this, I'd say that using LLMs to generate code is most likely to be useful if
1. You understand the problem well
2. You don't mind doing a lot of debugging
3. You don't want to start from an empty page.

You're not likely to save much time overall, since although you can get a first draft of your code almost instantly, the extra time you spend debugging it will likely consume the time you saved that way - especially since not having written the code yourself, you won't understand it as well as if you did. Unfortunately, vibe coding appeals particularly to people who want to do things quickly and cheaply, and aren't necessarily technical experts themselves.

by [Dr Peter J Bleackley]({% link index.md %})
