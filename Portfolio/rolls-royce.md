title: Case Studies - Rolls Royce AI Hub

#Rolls Royce AI Hub

## The Client

[R<sup>2</sup> Data Labs](https://www.rolls-royce.com/products-and-services/r2datalabs.aspx)

## The Problem

Rolls Royce had a large quantity of technical documents which they wanted to be able to search. They wished to develop their own search system in house, partly for security reasons and partly to ensure that it was optimal for their needs.

## The Approach

A testbed was developed to compare the performance of various topic modelling algorithms for searching the documents. During this work, a bug was found in the [Gensim implementation of TF-IDF](https://radimrehurek.com/gensim/models/tfidfmodel.html) and corrected. It was then necessary to develop a parser library that could extract structured data from various document formats. Many of the documents were scanned PDFs for regulatory reasons, and this led to two problems. Firstly, the OCR program used could infer the physical structure of the document (pages, layouts), but it was necessary to develop heuristics to infer logical structure (chapters, sections, paragraphs). Secondly, it was found that tables confused OCR. A method to handle this was developed in collaboration with another contractor, whereby tables would be separated into individual cells, OCR run on each cell, and the results assembled into a Pandas DataFrame. Methods were developed to account for row and column headers, as well as multirow and multicolumn spans.

During this project I also sat on a tender panel to advise on technical aspects of the bid and gave advice on a proposed collaborative project.

## Technology Used

* [Gensim](https://radimrehurek.com/gensim/index.html)
* [Poppler](https://poppler.freedesktop.org/)
* [Pandas](https://pandas.pydata.org/)
* [OpenCV](https://opencv.org/)

by [Dr Peter Bleackley]({% link index.md %})

[Case Studies]({% link Portfolio/index.md %})


