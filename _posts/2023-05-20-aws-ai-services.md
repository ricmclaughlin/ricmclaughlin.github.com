---
layout: post
title: "AWS - AI & ML Services"
description: ""
category: posts
tags: [ai-services, aws-solutions-arch-pro, aws-spec-ml, serverless]
---
{% include JB/setup %}

# Overview
There are two types of AI/ML services on AWS: 1/ AI services which are SaaS-style APIs that enable developers to do image process, NLP, forecasting and lots of other artificial intelligence tasks. 2/ Machine Learning services to enable data engineering, model creation, and inference tools in the Sagemaker family of services.

# AI services
* Rekognition - Rek is image recognition as a service that enables developers to find objects, text, and scenes in images. Facial recognition is possible for facial search of existing or a database of celebrities. Useful for labeling, content moderation, text detection, face - detection, analysis search, verification &amp; analysis. Analyzes faces using images stored in a _collection_ using the `IndexFaces` operation; can also be used to search for faces in a video stream using `CreateStreamProcessor`.

* Transcribe - user Automatic Speech Recognition (ASR) to convert speech to text while removing PII using Redaction, called Vocabulary filtering. Vocabulary filtering can use an existing word collection, like offensive_words, or a custom filter and then either mask or remove the offending word. Transcribe automatically understands multiple languages. Useful for transcription of calls, close captioning, and generating metadata for search. 

* Polly - text to speech service. Pronunciation lexicons enable the generation of speech from stylized words &amp; acronyms using the `SynthesizeSpeech` operation. Speech Synthesis Markup Language (SSML) enables more customization like word emphasis, phonetic pronunciation (using  pronunciation lexicons), breathing, and Newscaster speaking style.

* Translate - translates text between languages

* Lex - Lex is the same technology that powers Alexa using ASR. Lex uses the same terminology as skills - slots are spots where there are a set of options; a slot value is one of the options 

* Comprehend - NLP as a service that enables finding key phrases, places, or events, _understanding sentiment_, and organizing a collection of text files by topic. Use *Custom Entity Recognition* to identify tokens not defined by the service natively.

* Medical Comprehend - NLP as a service for medical use cases which detects PII

* Forecast - forecasting as a service using AutoML, automatic model selection. Add time series data augmented with prices, discounts, and other significant factors to generate 50% more accurate than just looking at the data. For scenarios where data is missing, Forecast can fill in missing data using middle, back, and future full techniques.

* Kendra - ML powered document search service that enables natural language search capabilities similar to google.com. Also has incremental learning capabilities so it learns from interaction and feedback and all of these features can be fine tuned.

* Personalize - recommendations as a service. Can use historical data, real-time data or both. Real-time data via SDK, CLI, or Amplify.  

* Textract - optical character recognition as a service; extracts text, handwriting, and data from scanned documents

* CodeGuru - supports automated code reviews in Java and Python and application performance review. CodeGuru Reviewer does static code analysis on commit while Profiler analyzes application performance supporting AWS and on-prem deployments with minimal overhead.

* Macie - ML/NLP processing service to discover, classify, and protect data on S3 using EventBridge notifications. Can detect PII, use CloudTrail logs, and is great for PCI-DSS.

* Amazon Augmented AI (A2I) enables human review based on the Confidence Level using Threshold reported for an inference'd item. The reviews are consolidated using weighted scores and stored in S3.

* Panorama - a machine learning appliance and software development kit (SDK) that enables computer vision to on-premises cameras to make predictions locally with high accuracy and low latency.

## ML Services
* Sagemaker - fully managed Jupyter notebooks with lots of other ML features.