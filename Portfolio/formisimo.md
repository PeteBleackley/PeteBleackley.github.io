---
layout: default
title: Case Studies - Formisimo
---

# Formisimo

##The Client

[Formisimo](https://www.zuko.io/formisimo)

## The Problem

Formisimo wanted to predict in real time whether users would complete or abandon web forms, in order to generate nudges that would encourage frustrated users to complete the form. Their early models were able to predict from the full history of user interactions whether a given user had completed or abandoned the form, but could not reproduce this under a simulation of real time operation.

## The Approach

After some initial experiments models based on Support Vector Machines and Hidden Markov models, a deep investigation of the data was made. It was found that a useful prediction of whether a user would complete the form or not could be made only within the last 100 interactions. It was therefore decided to change the prediction from whether or not the user would complete the form to whether the user was within 100 events of abandoning the form. Models based on this insight showed improved performance, and further improvements were made by using a LSTM model.

## The Outcome

New system was able to make useful predictions in real time.

## Technology Used

* [Scikit-Learn](https://scikit-learn.org/stable/) (Support Vector Machines)
* [Hidden Markov Models](https://pypi.python.org/pypi/Markov)
* [Keras](https://keras.io/) (LSTM networks)

by [Dr Peter Bleackley]({% link index.md %})

[Case Studies]({% link Portfolio/index.md %})
