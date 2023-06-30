---
layout: post
title: "Machine Learning - Algorithms"
description: ""
category: posts
tags: [ml, modeling, aws-spec-ml]
---
{% include JB/setup %}

## Logistic Regression
A binary classification algorithm with the X axis being continuous data and y being categorical data. The results of this sort of logistical regression is a Sigmoid function.

## Linear Regression &amp; Stochastic Gradient Descent
To compute the best fit for a linear regression, algos use loss functions. Loss function (cost function) - evaluation of the difference between predicted values and real values. Basic loss functions include Residual Sum of Squares (RSS), an approach that is largely brute force and im-practical at scale. Instead, a better approach is to use a stochastic gradient descent (SGD); in this approach, the algorithm samples along the gradient.

### Linear Learner
Expects normalized, shuffled data. Uses SGD and L1/L2 regularization

## Support Vector Machines (SVM)
SVM enable binary classification assignment. The points in each classification that are closest together are the _support vectors_ and the optimal hyperplane is the mid-point between the support vectors. The SVM with Radial Basis Function (RBF) kernel is a variation of the SVM (linear) used to separate non-linear data. This is implemented as "Factorization Machine" on Sagemaker. 

## Random Forests 
Useful for anomaly (&amp; outlier) detection; implemented in QuickSite, Kinesis Analytics, SageMaker
1. take random sample of data (using _bootstrapping_ is a dataset of the same size randomly selected from original dataset; the data left behind is the _out-of-bag (OOB)_ dataset) and then
take random features and then
2. create many trees
3. Tests with OOB samples to generate the _out-of-bag error_ rate to adjust the 
4. Vote for predictions

## K-means Clustering 
unsupervised; group dataset into K classification; ideally K is the knee in the total variation
1. decide how many clusters (K)?
2. Randomly assign K number of samples to a different cluster
3. for each sample, determine the nearest cluster
4. find center point (and repeat 3) 
5. measure the total variation (total length of center points of clusters to center point)

## K-Nearest Neighbors
supervised; answers: given K closest neighbors how should a new data point be classified?; optimally K is best determined by experimentation and use caution if the value of K gets near the number of samples in a class

# Text Algorithms
## Latent Dirichlet Allocation (LDA)
Topic analysis on a bag of words (a corpus of documents); a document can include one or more topics; a topic is a distribution of words

## Principal Component Analysis
Principal Component Analysis (PCA) (U) - is the combination of highly correlated data into a fewer number of features; it isn't dropping columns that are useless/weak relationship with label/target. This is done by finding a new set of features called components, which are composites of the original features that are uncorrelated with one another. PCA is commonly used as an exploratory data analysis tool.

