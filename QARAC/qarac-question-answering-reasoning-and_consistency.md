title: QARAC: Question Answering, Reasoning and Consistency

#QARAC: Question Answering, Reasoning and Consistency

Following on from my previous article on [The Future of Natural Language Processing]({% link future-of-nlp.md %}), I've decided to start start a personal research project to put some of these ideas into practice and test them out. 

I'm calling the proposed system **QARAC**, which stands for *Question Answering, Reasoning and Consistency*.

##NLP Components and Training Objectives

The main NLP components of the system will be two *encoders* and a *decoder*. The two encoders will share a base model, and each will map a sentence **S** to a vector *v*. One will be be a *question encoder*, $\mathcal{QE}$ and the other an *answer encoder*, $\mathcal{AE}$.

The *decoder* $\mathcal{D}$ will be an autoregressive model that, given a vector *v* generates a sentence **S**. In particular, it will be trained to act as the inverse function to the answer encoder, so that $$\mathcal{D}(\mathcal{AE}(\mathbf{S})) = \mathbf{S}$$. Further training objectives give the system its name.

###Question Answering

Given a question **Q** and a corresponding answer **A**, the *Question Answering* objective is that $$\mathcal{QE}(\mathbf{Q}) = \mathcal{AE}(\mathbf{A})$$. We might naively try to use this to create a simple question answering system as $$\mathbf{A} = \mathcal{D}(\mathcal{QE}(\mathbf{Q}))$$, but this of course would be no more likely to produce accurate results than current LLMs.

###Reasoning
Given two propositions $\mathbf{P_{0}}$ and $\mathbf{P_{1}}$, and a conclusion **C** that follows from them, the *Reasoning* objective is that $$\mathcal{D}(\mathcal{AE}(\mathbf{P_{0}}) + \mathcal{AE}(\mathbf{P_{1}})) = \mathbf{C}$$. 

###Consistency
Given two statements $\mathbf{S_{0}}$ and $\mathbf{S_{1}}$, the *consistency objective* is $$\mathit{cossim}(\mathcal{AE}(\mathbf{S_{0}}),\mathcal{AE}(\mathbf{S_{1}})) = \left\{ \begin{array}{c 1}
+1 & \quad \textrm{if statements are consistent} \\
0 & \quad \textrm{if statements are unrelated} \\
-1 & \quad \textrm{if statements contradict}
\end{array}
\right. $$

##Knowledge base components.

As previously stated, the system will need a knowledge base in order to produce accurate answers. This will be stored in a vector database and harvested by a crawler.

The crawler will start from a site considered likely to be a reliable source of factual information, extract statements from each document it crawls, end encode them with the answer encoder. It will then test them for consistency with the existing knowledge base, deciding on that basis which to add to the knowledge base and which to reject. It will also calculate an overall reliability score for each document. Links originating from documents with high reliability scores will be prioritised by the crawler for further investigation, and the crawler will terminate when there are no links left to be explored that come primarily from reliable sources.

##Querying

Presented with a question, QARAC will first use the question encoder to obtain a query vector. It will then find the top few matching vectors from the knowledge base, and use the cosine similarity of the answer vectors to the query vector used as a measure of confidence. If two vectors can be added to produce one with a higher confidence score, this will be added to the results set as an inferred answer. The answer vectors will then be converted to text by the decoder, and the results present to the user, showing the sources of the original vectors and the chain of reasoning to the inferred ones.

##Assessment
Well, that's the theory. This is a research project, however, and the point is to see how well this system performs in practice, and whether it provides insights into how NLP models could be further improved. As such, a demonstration system will be made accessible, and feedback solicited from users about its performance.

Code for the projects will be published on [GitHub](https://github.com/PeteBleackley/QARAC) and trained models on [HuggingFace](https://huggingface.co/PlayfulTechnology). 

by [Dr Peter J Bleackley]({% link index.md %})

[QARAC]({% link QARAC/index.md %})
