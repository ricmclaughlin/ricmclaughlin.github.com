---
layout: post
title: "Machine Learning - Data"
description: ""
category: posts
tags: [ml, aws-spec-ml]
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

# Other Data
## Missing Data
There are three approaches to missing data:
Ask the domain expert
Delete the data
Impute the data using a mean or regression.

## Useless Data
Useless data is data that has no statistical relevance to the problem being solved. Often useless data is called "high cardinality" data because it has no relationship to the label or other inferred value. To determine if the data is useful/useless ask a data domain expert or do a data study to determine any sort of correlation. Common useless data includes:
* Indexes 
* Unique identifiers (like Account numbers)
* Names, Email address

## Unbalanced Data
Unbalanced data is data that lacks, important rare data.
There are three approaches to missing data:
0. Ask the domain expert
0. Choose the right measure of success - accuracy is less the point of unbalanced data; sensitivity and specificity, or the combined score of these elements an F1 score, are better measures.
0. undersample, oversample, or synthesis - undersampling is simply removing some of the overweighted class of data; oversample is including more of the underweighted data (which can be implemented using SMOTE - Synthetic Minority Oversampling Technique - in Data Wrangler); synthesis is just creating some sample data. 

