---
layout: post
title: "AWS - AI & ML Services"
description: ""
category: posts
tags: [ai-ml-services, aws, aws-solutions-arch-pro]
---
{% include JB/setup %}

# Overview
There are two types of AI/ML services on AWS. AI services which are SaaS-style APIs that enable developers to do image process, NLP, forecasting and lots of other artificial intelligence tasks. There are also a large family of Machine Learning data engineering, model creation, and inference tools in the Sagemaker family of services.

# AI services

Confidence Level and Threshold reported for item, item is flag in Amazon Augmented AI (A2I)

Rekognition - Rek is image recognition as a service that enables developers to find objects, text, and scenes in images. Facial recognition is possible for facial search of existing or a database of celebrities. Useful for labeling, content moderation, text detection, face - detection, analysis search, verification &amp; analysis. Analyzes faces using images stored in a _collection_ using the `IndexFaces` operation; can also be used to search for faces in a video stream using `CreateStreamProcessor`.

Transcribe - user Automatic Speech Recognition (ASR) to convert speech to text while removing PII using Redaction and automatically understanding multiple languages. Useful for transcription of calls, close captioning, and generating metadata for search.

Polly - text to speech service. Pronunciation lexicons enable the generation of speech from stylized words &amp; acronyms using the `SynthesizeSpeech` operation. Speech Synthesis Markup Language (SSML) enables more customization like word emphasis, phonetic pronunciation, breathing, and Newscaster speaking style.

Translate - translates text between languages

Lex &amp; Connect - Lex is the same technology that powers Alexa using ASR. Connect is a call center as a service which is 80% cheaper than tradition contact center solutions. Combining Lex and Connect together enables chatbots and call center bots.

Comprehend - NLP as a service that enables finding key phrases, places, or events, understanding sentiment, and organizing a collection of text files by topic.

Medical Comprehend - NLP as a service for medical use cases which detects PII

Forecast - forecasting as a service. Add time series data augmented with prices, discounts, and other significant factors to generate 50% more accurate than just looking at the data.

Kendra - ML powered document search service that enables natural language search capabilities similar to google.com. Also has incremental learning capabilities so it learns from interaction and feedback and all of these features can be fine tuned.

Personalize - recommendations as a service; 

Textract - optical character recognition as a service; extracts text, handwriting, and data from scanned documents

CodeGuru - supports automated code reviews in Java and Python and application performance review. CodeGuru Reviewer does static code analysis on commit while Profiler analyzes application performance supporting AWS and on-prem deployments with minimal overhead.

Macie - ML/NLP processing service to discover, classify, and protect data on S3 using EventBridge notifications. Can detect PII, use CloudTrail logs, and is great for PCI-DSS.

## ML Services

Sagemaker - fully managed Jupyter notebooks with lots of other ML features.