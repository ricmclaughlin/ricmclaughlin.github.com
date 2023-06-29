---
layout: post
title: "Machine Learning - Modeling"
description: ""
category: posts
tags: [ml, aws-spec-ml]
---
{% include JB/setup %}

# Model types
Models can be supervised, where the data is labeled, and unsupervised where the data is not labeled.

There are three types of ML models:
- Binary classification - predicts values that can only have two states
- Multi-class - group into a defined set of classification option (Multinomial Logistic Regression)
- Regression - predict a numeric value

# Selecting the Right Model
There are numerous models built-in to SageMaker and some are even pre-trained. Here are the algorithms supported by SageMaker, by data type by use case. Legend: S = Supervised; U = Unsupervised; T = Text; I = Image/video

## Tabular algorithms by Use Case
### Classification and Regression
All these can be used for binary/multi-class classification AND regression of a numeric or continue value using a supervised model.
- K-Nearest Neighbors - simple classification or regression; input: K!
- Factorization Machine - works well with sparse data; pairs of data (user -> page); click predictions and item recommendations; 
- XGBoost uses decision tree; takes CSV and libsvm; memory bound; GPU training on single instance using `gpu_hist`
- Linear learner for labeled data classification. RecordIO/Float32; file or pipe mode

### Reduce dimensionality
- Object2Vec (DL) - enables embeddings, the conversion of high-dimensional objects into low-dimensional space. Example: identify duplicate support tickets or find the correct routing based on similarity of text in the tickets; single instance or single/multiple GPU instances; P3 based inference.
- Principal Component Analysis (PCA) (U) - it isn't dropping columns that are useless/weak relationship with label/target, it is the combination of highly correlated data into a fewer number of features; RecordIO/CSV using file or pipe mode

### Time-series forecast 
- DeepAR forecasting (S) - time-series predictions based on historical data for a behavior, predict a future behavior across multiple time series streams. Input all data in JSON (can be in Parquet). Start with CPU instances for training; multi-machine or single; can use GPU for larger models or large mini-batch sizes (>512). Prefer DeepAR over Autoregressive Integrated Moving Average (ARIMA) and Error, Trend, Seasonality (ETS) forecasts.

### Anomaly Detection 
- Random cut forest (RCF) (U) - Detect abnormal behavior in application: spot when an IoT sensor is sending abnormal readings; input: RecordIO-protobuf or CSV using pipe or file mode; training/inference: no GPU required

### Detect intrusion by IP
 - IP Insights (U) - used to protect apps from suspicious users by IP address; inputs CSV including usernames and AccountIDs; 1 to many GPU for training

### Clustering or grouping 
- K-Means (U) - Group similar objects/data together: find high-, medium-, and low-spending customers from their transaction histories; input: RecordIO-protobuf or CSV using pipe or file mode; CPU inference/training 
        
### Topic modeling 
Organize a set of documents into topics (not known in advance): tag a document as belonging to a medical category based on the terms used in the document; `num_topics` is the definition of how many topics to find for both algos

- Latent Dirichlet Allocation - unsupervised; input: train channel, optional test channel using RecordIO-protobuf or CSV using pipe or file mode containing counts for each word in vocabulary; single CPU based inference/training (so likely cheaper)

- Neural Topic Model (NTM) - unsupervised - 4 channel input: train channel of RecordIO-protobuf or CSV using pipe or file mode containing tokenized integers and a dictionary (the aux channel)
    
### Recommendations
- Collaborative filtering - part of Tensorflow 2.0 uses user, item, rating and similarity to other users to create recommendations. Content filtering is similar but not as effective.

## Text analysis algorithms
### Classify text 
BlazingText (S) - base use case is to assign pre-defined categories to sentences in a corpus; Training data must be labeled; input is one sentence per line with `__label__` and label prepended or "augmented manifest text format". Can also implement word2vec for word embedding with tokenized input; has three modes *continuous bag of words*, skip-gram, and batch skip-gram (for distributed computation)

### Language translation, summarization, speech to text 
Sequence to Sequence (seq2seq) - input tokenized text file, convert to protobuf; requires vocabulary file; can optimize on accuracy, BLEU score, perplexity; DL so requires GPU; single instance only but does support multiple GPU
    
## Images classifications algorithms
### image and multi-label classification (image overall)
MXNet or TensorFlow - Label/tag an image based on the content of the image: alerts about adult content in an image; MXNet includes full training mode and transfer learning model (initialize with pretrained weights, top layer with random weights, fine tune with new data); TensorFlow allows the top layer to be trained for finer tuning; GPU/instances multiple/single for training; M class inference possible likely needs GPU

### object detection
detect specific people or objects (bounding box) - MXNet or TensorFlow - Detect people and objects in an image: police review a large photo gallery for a missing person; MXNet input: RecordIO; GPU/instances multiple/single for training

### detailed pixel-level tagging
Semantic Segmentation - computer vision (tag pixels into a segmentation mask); input is JPG and PNG ideally in Pipe mode. Built on MXNet; can be trained incrementally. GPU required on single instance for training; inference on CPU or GPU

# Training

## Transfer Learning
Transfer Learning saves time using pre-trained models; often called foundational models. Four approaches:
- Fine tune with more data - using the same data type and input size and a low learning rate; network is initialized with pre-trained weights and the output layer is initialized with random weights
- Add another layer - freeze weights then add new a layer. (fine tuning and layer adding works too)
- Retrain from scratch - giant amount of work
- Use as is

# Hyperparameters
Hyperparameters control how a model is trained - they are the switches, nobs, and dials. Common hyperparameters include:

- epoch count - the number of times the data is passed through the algorithm
- batch size - the size of a subset of the training dataset to process at a time
- mini-batch - the amount of a batch sent to a compute environment in the training fleet
- learning rate - how big a step the model should take when executing optimization
- optimizer - which algorithm to use for optimization; Gradient descent, Stochastic Gradient decent, and ADAM are common choices with ADAM the most most common choice.
- predictor type - Specifies the type of target variable as a binary classification, multiclass classification, or regression.

Generally, the learning rate should be increased as the mini-batch size increases.

If the control of the model is less important, automatic hyperparameter optimization can be used - likely implemented as an AI process. Enabling *early stopping* can help avoid overfitting using an objective metric evaluated at the end of each epoch using TensorFlow, MXNet, Chainer, PyTorch, and Spark.

## Regularization
Regularization is desensitizing the algorithm to the data to prevent over-fitting by adding bias to the model using a regression penalty. Regularization is used mitigate over-fitting models and when the dataset has more features than samples (the data is wider than it is long). 

There are two types of regularization that are applicable to non-DL models:
- L1 (Lasso) which eliminates useless features to generate sparse output; computationally inefficient
- L2 (Ridge) which reduces the influence of some of the features. Computationally efficient; assumes all features are important. Also called *weight decay*

Use L2 regularization when there are more features than samples.

### Regularization for DL
Several approaches for regularization in DL:
- Remove layers, decrease neurons
- Dropout - drops random number of neurons each epoch (common in CNN) - Dropout layers force the network to spread out its learning throughout the network, and can prevent over-fitting resulting from learning concentrating in one spot.
- Stop early - detects drop in validation dataset accuracy then stops

# Evaluation
Bias is how tight the model fit is; low-bias is over-fit.
Variance is how much predictions vary from actual data; high-variance is over-fit

Easiest way to think about this:
- under-fit = high bias/low variance; too cold
- generalizes well or balanced = moderate bias and variance; just right
- over-fit = low bias/ high variance; too hot

Ideally, models balance bias and variance. There is an inverse relationship between bias and variance.

## Confusion Matrix
A confusion matrix is a tool for visualizing the performance of a single or multi-class model.

|        |    |       | reality             |               |
|--------|----|-------|---------------------|---------------------|
|        |    |       | TRUE                | FALSE               |
| Model  | >> | TRUE  | True positive (TP)  | False positive (FP) |
| output | >> | FALSE | False negative (FN) | True negative (TN)  |

Accuracy = TP + TN / Total
Recall = TP / (TP + FN)
Precision = TP / (TP + FP)

Specificity = TN / (TN + FP)
False positive rate = 1 - specificity

### Accuracy
Does the model make correct inferences?

Accuracy is number of correct predictions / # of predictions. For accuracy, the # of correct predictions is TP + TN. This measure requires balanced data, data where TP and TN both occur with some regularity.

### Recall
Recall (true positive rate or sensitivity) is TP / (TP + FN); ranges from 0 to 1, where higher is better; measures actual positives measured as positives. In other words, how well does the model find fraud, assuming that it's ok to find non-fraudulent transactions.

### Precision
Precision (positive predictive value) is TP / (TP + FP); ranges from 0 to 1, where higher is better; measures actual positives from predicted positives. In other words, how well does the model do at making sure that content is safe for children, assuming we are ok if lots of content that is safe is left out.

### F1
F1 is the harmonized average of precision and recall
F1 = 2 ((precision * recall) / (precision + recall))

### Specificity & False Positive Rate
Less commonly metrics include specificity which is TN / (TN + FP). The false positive rate is 1 - Specificity. This is the *false alarm rate* so lower is better!

## ROC & AUC
ROC (Receiver Operating Characteristic) is useful for providing data on which model works best for the requirements. ROC curve is a graphical plot that shows the diagnostic ability of a binary classifier system as its discrimination threshold is varied.

AUC (Area under the curve) AUC measures the ability of the model to predict a higher score for positive examples as compared to negative examples. AUC is useful for understanding how effective the algorithm is at classifying data; the higher the better; a value of 1 is a unicorn un-achievable score. 

## Triage
### Effectiveness
accuracy - given balanced data, does model work?
recall - How many of the positives got predicted? use when cost of FN is high
precision - How many the positive predictions were correct?  use when the cost of FP is high
false negative rate

Correlation coefficient - negative = poor correlation; positive = good correlation; zero = no correlation; 1 = perfect positive correlation; (AKA Pearson's correlation coefficient); Bayesian for dependent variables; naive Bayesian for independent variables

### Fit
Getting good data about the situation: Use AUC metrics against hyperparameters on a scatter chart to evaluate the best model version.

DL: Recall low/FN important? add weight to FN

DL Precision low/FP important? add weight to FP.

Good training, bad validation? Over-fit

Poor training, poor validation? Add training data, increase passes

Over-fit - use fewer features, decrease n-gram size, increase regularization (L2); in a neural net, a Dropout, early stopping, adding noise can be used to regularize.

under-fit - add domain specific features, Cartesian products, increase n-grams size, decrease regularization (L2); in a neural net, increase amount of data OR increase epochs OR increase depth of network

Neural network, loss function oscillating - learning rate too high.

Neural network, loss function slowly decreasing - learning rate too low.

### Model Specific
Regression? Mean Absolute Percentage Error (MAPE) OR Root-mean-square error (RMSE)

Classification threshold analysis? ROC/AUC; use `scale_pos_weight` to adjust the discrimination threshold.

Training k-NN, low precision, low accuracy? data needs normalization

Time-series analysis - trend can be uptrend, downtrend, sideways; seasonality is regular changes that happen annually

Reduce size of DL model - model pruning removes weights that don't contribute much to the overall model accuracy. Activation functions do not effect model size. SageMaker Debugger to do this.



