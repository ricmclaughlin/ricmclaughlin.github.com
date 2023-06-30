---
layout: post
title: "AWS - Sagemaker Family of Services"
description: ""
category: posts
tags: [mlops, aws-solutions-arch-pro, aws-spec-ml]
---
{% include JB/setup %}

# AI/ML Services Overview
There are two classes of AI/ML services on AWS: 1/ Machine Learning services to enable data engineering, model creation, and inference tools in the Sagemaker family of services. 2/ [AI services]({{ BASE_PATH }}/posts/aws-ai-services) which are SaaS-style APIs that enable developers to do image process, NLP, forecasting and lots of other artificial intelligence tasks.

Both of these classes of AI/ML are briefly covered on the Solution Architect Pro Certification and in-depth on the AWS Machine Learning certification. 
    
# Sagemaker Notebooks
Fully managed Jupyter notebook on an EC2 instance with lots of other ML features. Uses an instance role, optionally runs in a VPC; integrates with Git and KMS. Has the ability to run a lifecycle configuration script that is executed on start/stop and initial boot.

## Spark access
The SageMaker Spark library enables integration with built in `sagemaker-spark` library enabling off cluster training using Spark and then do inference using SageMaker end points. Can use K-means, PCA &amp; XGBoost models from SageMaker using this mechanism. Seems to be accessible from Notebooks and Studio.

# SageMaker Studio
Instead of using Jupyter notebooks, SageMaker studio is made on Jupyter Labs. Includes SageMaker experiments to manage hypothesis and tests. 

## GroundTruth
GroundTruth is a data labeling solution where humans label data. It enables people management using Mechanical Turk or team mates. The data can be images (single label, multi-label, bounding box, semantic segmentation, or label verification for QA), text, or video objects/video tracking (classification, bounding box, ploygon, polyline, keypoints). GroundTruth can be configured to only sends ambiguous data to labelers and improves over time. The output of a GroundTruth job is a manifest file.

## SageMaker Debugger 
This feature saves the model state at periodic intervals and integrates CloudWatch events to monitor system bottlenecks, profile model operations, and debug model parameters. Supports most built in DL frameworks with extensibility with the `CreateTrainingJob` and `DescribeTrainingJob` APIs with access via the `SMDebug` client library. Also included is a dashboard and notifications; can optionally cease the training.

## Clarify
Transparency on how models arrive at predictions through feature attribution where each feature is given an importance value for a given prediction.

Clarify can also generate bias metrics pre-training. Biases that can be detected include lots of facet and outcome variance metrics. 

## Model Monitor
Get alerts on quality deviations on deployed models via CloudWatch. This feature enables data drift, detect anomalies &amp; outliers, and can detect new features. Model Monitor &amp; Clarify integrate; monitoring jobs are scheduled with data storage in S3. Integrates with Tensorboard, QuickSight, Tableau &amp; SageMaker Studio. Model Monitor can detect:
- Data quality drift
- Model quality drift (accuracy, recall, precision) and work with GroundTruth to detect changes between model and real human classification.
- Bias drift
- Feature attribution drift

## Amazon SageMaker Data Wrangler
Enables import, transform, analysis and export of data using custom Python code. Fills same role as Glue Data Brew; Data Brew is more no-code focused.

## Autopilot (AutoML)
AutoPilot automates the feature engineering, algorithm selection (binary, multiclass, or regression types), and model tuning process as well as tests multiple algorithms and multiple variations of each algorithm, then it will select the one that best fits the data; only supports tabular data. Input data needs to be CSV or Parquet; models include Linear Learner, XGBoost, DL and an ensemble mixed mode. Integrates with Clarify. Does not directly support using pre-trained models and fine-tuning them.

Using trial and error, AutoML selects algo, data preprocessing, model tuning, and all infrastructure. Loads data from S3 (CSV/Parquet), select a column for prediction, and then AutoML creates a model leaderboard. From here deployment or refining the output via a notebook is the next step.

# Inference
Can occur real-time or batch. For real-time, first create a model, a packaging exercise not a model training exercise, to include the ECR image (if the training job used a SageMaker supplied algorithm) then next create an inference endpoint.

By default SageMaker endpoints are not public; use API Gateway to expose it.

# Training
Both Notebooks and Studio can use [built-in algorithms](https://docs.aws.amazon.com/sagemaker/latest/dg/algos.html), customer supplied algorithms, customer supplied ECR image, Marketplace algorithms.

High level process: 
- Set hyperparameters which differ by model type

- configure data sources including input train and validation channel (training dataset and test dataset) and returns bucket for the model; spot instances can be used for training

- Built in training models generate CloudWatch metrics (at a 1 minute resolution) for the algorithm (including accuracy) and instances. The job also generates logs. This doesn't work by default for custom libraries and has to be implemented using a CloudWatch compatible library.

- SageMaker supports local training using the pre-built TensorFlow and MXNet containers. 

## Automatic Model Tuning (AMT)
AMT uses the algorithm and ranges of hyperparameters to create a model that performs the best based on the chosen metric and it learns as it goes. By default AMT assumes all hyperparameters are linear-scaled and over time will discover if hyperparameters are log-scaled; save time by figuring out which are log-scaled and set them as such.

There are 4 types of AMT:

- Grid search - hyperparameter tuning chooses combinations of values from the range of *categorical values* that specified during job creation
- Random search - random combinations; works well with lots of concurrency
- Bayesian Optimization - treats tuning like a regression learning incrementally on each pass; isn't great at parallel learning so a smaller number of concurrent tuning jobs is likely more cost/time efficient.  

- Hyperband - dynamically reallocates resources. Hyperband uses both intermediate and final results of training jobs to re-allocate epochs to well-utilized hyperparameter configurations and automatically stops those that underperform. It also seamlessly scales to using many parallel training jobs.

[A Machine Learning Specialist is optimizing values for a model’s learning rate using Amazon SageMaker automatic model tuning. The specified range of values for the learning rate is between .0001 and 1.0. The tuning time was more expensive and slower than expected.
    
Set the scaling type of the ContinuousParameterRanges parameter to Logarithmic and adjust the desired minimum learning rate into a value greater than 0.]

## Compute
Training compute resources can be configured to be network isolated (ML MarketPlace products require Internet access), can be configured to run in a VPC, and can be configured to use TLS/SSL between containers.

Use the NVIDIA Container Toolkit to run GPU accelerated docker containers, called `nvidia-docker` compatible containers.

Managed Spot Training uses Spot to run training jobs, managed Spot interruptions; need to specify a checkpointing mechanism.

## SageMaker Built in Algorithms
There are lots of built in algos as documented on the modeling section of these notes.



