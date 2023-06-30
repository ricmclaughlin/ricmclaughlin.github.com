---
layout: post
title: "Machine Learning - Neural Networks"
description: ""
category: posts
tags: [modeling, aws-spec-ml]
---
{% include JB/setup %}

# Neural Networks

## Activation Functions
Activation functions are how the computation happens and enables back propagation. Choosing the right one is an art and largely based on the type of network. However, using use softmax as the output layer; it converts a vector of real numbers into a probability distribution with a SUM of 1, is a best practice.

## Convolutional Neural Networks (CNN)
Convolutional Neural Network are for *Image analysis* to find "feature-location invariant" pattern by breaking down into small pieces (convolutions). Resource intensive with lots of hyperparameters. There are specialized CNN architectures including LeNet-5 for handwriting, AlexNet for image classification, GoogLeNet (deeper better performance using groups of convolution layers called inception modules), and ResNet (even deeper).

### How CNN work
filters (or kernel) generates a feature/activation map by examining the pixels in framed part of the image. The filter sizes are commonly 3x3, 5x5, 7x7. Because border pixels are evaluated fewer times they are distorted causing the *border effect*. To account for the border effect it is common to add padding around the edges of image. It's common to modify the layer stride - the stride is how many pixels to move the filter over during filter analysis; a larger stride decreases the size of the activation map. It's also common to add  Pooling layers to down samples image; map pooling is the most common approach; commonly uses a filter size of 2x2 and a stride of 2

For activation start with ReLU, shift to Leaky ReLU, then try PReLU or Maxout. Use Swish for really deep networks.

## Recurrent Neural Networks (RNN)
RNN are used on time-series data, but really it's any sequence of data in an arbitrary length. RNN do well with Tanh, a non-linear function for their activation function.

Standard RNN suffers from the vanishing gradient problem where words at the beginning of sequence loose significance. LSTM (Long Short-Term Memory) is a specific type of RNN that solves the problem of items losing their weight over time. A GRU (Gated Recurrent Units) is a type of RNN that uses far less processing than LTSM and may be faster to train.

## Transformers
Transformers use "self-attention" mechanism to weigh the significance of each part of the input. BERT is a family of transformers.




