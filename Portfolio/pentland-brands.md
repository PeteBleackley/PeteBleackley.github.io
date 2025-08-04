---
layout: default
title: Case Studies - Pentland Brands
---

# Pentland Brands

## The Client

[Pentland Brands](https://pentlandbrands.com/)

## The Problem

Pentland Brands wanted to create an app to recommend swimming goggles to potential customers. During a trial, they had collected point cloud models of test subjects' faces using the 3D scanner on an IPhone, along with metadata about the test subjects, and whether they liked or disliked various styles of goggles. They wished to know if it would be possible to predict whether a given person would like a particular style of goggles.

## The Approach

A test framework was created which allowed the performance of various models to be compared. A number of data reduction techniques and classifier algorithms were applied to the data and their performance in predicting the test subjects' preferences were assessed. Unfortunately, it was discovered that there was no significant correlation between facial shapes and preferences, so Playful Technology Limited recommended that the project be discontinued.

## The Outcome
The client saved the cost of further wasted effort.

## Technology Used

* [python-pcl](https://github.com/strawlab/python-pcl)
* [scikit-learn](https://scikit-learn.org/stable/)
* [Theano](https://pypi.org/project/Theano) (graph convolutional network)
* [Pandas](https://pandas.pydata.org/)
* [Jupyter Notebook](https://jupyter.org/)

by [Dr Peter Bleackley]({% link index.md %})

[Case Studies]({% link Portfolio/index.md %})
