---
layout: post
title: "Machine Learning - Data Engineering"
description: ""
category: posts
tags: [ml, aws-spec-ml, design-data]
---
{% include JB/setup %}

# Data Types
There are numerous types of data and each has an encoding mechanism that can improve ML results. 

## Binary
Binary is boolean data. It's pretty easy to determine binary data - the unique value count is 1, 2, or 3.

## Numerical - Continuous
Numeric is data that can be *measured* on a continuous quantified scale. It is also called interval data. Examples include: 
* Temperature
* Heart Rate
* Age

## Ordinal data
Order matters but the distance between the value is not quantified. Includes measures like ranking, ratings, &amp; range bins. Think: t-shirt size.

## Categorical
The range of values is not ordered; may includes things like color, species, and food preferences.

## Text data
Text data is words, sentences, or documents.

## Temporal data
Temporal data is date, time, or order data - time series. Time series data can form trends and can be seasonal OR both. Seasonality + Trends + Noise = Time series.

# Data Engineering
There are lots of different types of data for ML - and these data types are [pretty standard](). Once the data is assembled it needs to be prepared. Generally, data should be formatted for efficient loading - Pipe mode using RecordIO, which concats many images into a single file where the pipe will load a range of the file, instead of doing a file system load can improve IO. Commonly implemented for S3 data.

Generally, data should be randomized. It's likely not appropriate to randomize data if working with time-series data.

Generally, data should be split into training and testing date - for randomly sorted data split data into commonly 80% training, 20% testing or perhaps 90/10 if using a large data set. For time series, ask a subject matter expert about the split. When randomizing data with labels include the label with the data prior to randomization so that the label corresponds to proper data. 

## Data Engineering Techniques
### Binning
Placing continuous data into categories which both condenses and provides more meaningful data.

## Normalizing
Run the data through an algorithm that normalizes the values into a set range, typically normally distributed around 0; useful when datasets have different ranges and prevents the algorithms from being influenced by high values

## K-Fold Cross
K-Fold Cross enables the solution to use all the training data to test the model. Useful when evaluating multiple model candidates, to get better metrics, and particularly help when working with sequenced data. Process steps:
0. Divide the data set into K chunks (folds), where K is a number like 7.
0. Select a fold to be the test data and training the model
0. Continue training new models until all the folds have served as test data.

## PCA 
Principal Component Analysis (PCA) - is the combination of highly correlated data into a fewer number of features; unsupervised; it isn't dropping columns that are useless/weak relationship with label/target

## Embedding
Object2Vec can learn low-dimensional dense embeddings of high-dimensional objects to produce features that improve training efficiencies for downstream models. Using pre-trained word embeddings can improve training times and accuracy.

## Pipe mode
SageMaker comes with a faster, higher-IO *Pipe mode* implementation that significantly accelerate the read speeds from S3 for data stored in protobuf RecordIO format. Using *file mode* SageMaker copies the file to each training worker. Pipe mode is faster than any EBS scenario.

## Compressed data format 
For more compact storage and faster queries, use Parquet or ORC as the storage format. 

## Change data capture (CDC)
Replicate relational data to S3 - Use DMS task for both full load and change data capture (CDC) data from relational databases using the CSV format by default.

### Oversampling 
Typically done by copying data that is already present. However, SMOTE is a better way to oversample. Creates new samples from K-nearest-neighbors of both majority and minority classes.

### Undersampling
Undersampling is good for reducing amount of data overall to relieve too-much-data issues, generally not good though (never good to have too little data)

### Data synthesis
If data is missing, mean or medium replacement of missing data is fast but isn't a great solution (can miss correlation, outliers can be skew), row dropping likely bias data, K-nearest neighbors (KNN) takes an average of rows like the row with missing data (great for numerical data), Deep learning (great for categorical data), regression (MICE is an advanced great technique). Ideally getting more data is the best fix. 

### Binning
Binning, or creating a feature that creates a set of ordinal groups, smooths over in-accurate data. Quartile binning places an even number in each bin.

### Mathematical Transformation
Applying a mathematical function (square root or squaring) can enable learning of super or sub-linear functions.

### Tokenization 
Tokenization is simply creating a list of all the words in some text data. Commonly the next step is to create a _bag of words_  that includes the frequency of words. _Stop word_ removal strips common word from a bag of words. *Stemming* strips the contextual aspect of the word reducing it to the language stem so it can be connected together in the bag of words.

### NLP prep
N-gram Transformation for splitting sentences into words, Orthogonal Sparse Bigram (OSB) Transformation aids in text string analysis.

### Triage
When evaluating multiple models or when working with sequenced data, K-fold Cross enables the use of all data. 

Highly dimensional? Principal Component Analysis (PCA), t-Distributed Stochastic Neighbor Embedding (TSNE), and K-Means can be used to reduce the number of highly correlated features, or better said, useless data. Use Object2Vec to create neural embeddings.

Categorical data? one-hot encoding - good idea to convert categorical data should be converted into numerical forms

missing data? MICE, mean/median replacement, KNN

Prep NLP data? lower case, stop word removal, HTML removal, tokenization, stem

NLP low amounts of rich vocab? `TfidfVectorizer` - converts text data into its numerical representation that can be passed into a machine learning model; it penalizes generic words that commonly appear across all sentences. A Tf-idf matrix is [row, (# unigrams + # bigrams)]

Unbalanced data? undersampling by removing some of the overweighted class of data; oversample is including more of the underweighted data (which can be implemented using SMOTE - Synthetic Minority Oversampling Technique - in Data Wrangler); synthesis is just creating some sample data. 

in-precise data? Binning (then likely one-hot encoding)

non-zero mean continuous data? Standard scaling.

Exponential differences in numerical/continuous data OR Too many outliers? Normalization transformation will likely make training more effective. Very significant for k-NN.
- Skewed with long tail on right? Logarithmic normalization
- Skewed with long tail on left? Third-order polynomial transformation

Label data? - GroundTruth, Rek, Comprehend


