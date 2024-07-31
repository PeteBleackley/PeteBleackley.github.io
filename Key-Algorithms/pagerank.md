title: Key Algorithms - PageRank

#PageRank

Early web search engines, such as AltaVista, relied on hand-curated indexes of content. This was, of course, difficult to scale. What was needed was an automatic way of ranking web pages. Larry *Page*, developed an algorithm for *ranking* the importance of nodes in a network (such as web *pages*) in terms of their connections during his PhD at Stanford University, and then went on to found Google to exploit his research.

The *PageRank* algorithm is based on three assumptions
1. The more valuable a web page is, the more likely other web pages are to link to it.
2. Links originating from more valuable pages confer more value on the pages they link to.
3. Pages that link indiscriminately to many other pages confer less value on those pages than those which link more selectively.

Based on these assumptions, it then models a *random walk* taken through the Internet by a user clicking web links at random. If the user is viewing a web page $i$ that has $N_{i}$ outgoing links, they have a probability $d$ (known as the *damping factor*, and typically chosen as 0.85) of clicking a link to another page. This link is assumed to be chosen with uniform probability from the page's outgoing links. The PageRank $P_{i}$ for the page is a measure of how likely the page is to be found by this method.

If $L_{i}$ is the set of pages that link to $i$, the PageRank satisfies the equation

$$P_{i} = d \sum_{j \in L_{i}} \frac{P_{j}}{N_{j}} + (1-d)$$

This is solved iteratively.

If we define a *connection matrix* $\mathbf{C}$ such that $C_{ij}$ is $1/N_{j}$ if $j$ connects to $i$ and 0 otherwise, we can express this as a matrix equation

$$\vec{P} = d \mathbf{C} \cdot \vec{P} + (1-d)$$

We then see that the PageRank is a modified form of the first eigenvector of the connection matrix.

Like [Collaborative Filtering]({% link KeyAlgorithms/colaborative-filtering.md %}), PageRank is an example of a *collective intelligence* algorithm, in that it uses data from the actions of a large number of people to infer its scores.

PageRank is one of the most commercially successful algorithms ever devised, however its uses are not limited to ranking web pages. It can be used to analyse any data that can be modelled as a graph, such as citations in academic papers, patterns of gene activation in cells, or connection in the nervous system. A survey of these uses can be found in [PageRank Beyond the Web](https://www.cs.purdue.edu/homes/dgleich/publications/Gleich%202015%20-%20prbeyond.pdf)

by [Dr Peter J Bleackley]({% link index.md %})

[Key Algorithms]({% link Key-Algorithms/index.md %})

Collective Intelligence
: [Collaborative Filtering]({% link Key-Algorithms/collaborative-filtering.md %})
: *PageRank*

