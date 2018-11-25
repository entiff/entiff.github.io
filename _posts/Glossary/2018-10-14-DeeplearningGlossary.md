---
layout: post
title:  "딥러닝 용어 모음(Deep Learning Glossary)"
date:   2018-10-14 12:00:00
author: entiff
categories: Glossary
---

[DeepLearningGlossary](http://www.wildml.com/deep-learning-glossary/), 밑바닥부터 시작하는 딥러닝 등을 참고하여 딥러닝 용어 모음집을 작성 중입니다.  
여러분들은 아래의 용어에 대해 모두 설명할 수 있으신가요?  
아래의 용어들에 대해 모두 간단하고 쉽게 설명해보는 것을 목표로 공부하면 좋을 것 같습니다 :)  
오타 혹은 오개념에 대해서는 댓글로 남겨주시면 감사하겠습니다.  
지속적인 업데이트가 이루어지고 있습니다.  

피드백은 언제나 환영입니다.

## 1. Neural Net(신경망)

* Perceptron(퍼셉트론)
* Unit
* Node
* Layer(층)
* Backpropagation(역전파)
* Backpropagation through time (BPTT)
* Channel
* Nonlinearity(비선형)

### 1-1. Problems

* Vanishing Gradient
* Exploding Gradient
  - Gradient Clipping
* Overfitting(과적합)

## 2. Architecture

* CNN(Convolutional Neural Network, ConvNet, 합성곱 신경망)
  - Pooling(풀링)
    - Average-Pooling
    - Max-Pooling
  - Padding
  - Inception Module
* RNN(Recurrent Neural Network)
* Recursive Neural Network
* GRU(Gated Recurrent Unit)
* LSTM(Long Shot Term Memory)
* Attention Mechanism
* encoder, decoder
  - Auto-encoder
* Bi-directional RNN
* Seq2Seq

## 3. Learning technic

### 3-1. Optimizer

* Gradient Descent(경사하강법)
* Stochastic Gradient Descent(확률적 경사하강법)
* Momentum
* Adadelta
* Adagrad
* RMSProp
* Adam

### 3-2. Weight Initialization

* Xavier Initialization(LeCun Initializer)
* He Initialization

### 3-3. Activation Function(활성화 함수)

* Sigmoid Function
* ReLU
* Leaky ReLU
* tanh(hyperbolic tangent)

### 3-4. Batch Normalization

### 3-5. Regularization

### 3-6. Dropout

### 3-7. Augmentation

## 4. Loss Function(손실함수)

* Categorical Cross-Entropy Loss
* NLL(Negative Log Likelihood)

## 5. Softmax

## 6. Framework

* TensorFlow
* Pytorch
* Keras
* Caffe

## 7. Milestone models

### 7-1. Image Detection

* VGG
* Alexnet
* ResNet
* GoogleLeNet

### 8-1. GAN(Generative adversarial networks)

### 8-2. Neural Machine Translation

* Seq2Seq models
* RNN encoder-decoder

### 8-3. QA(Question & Answer)

### 8-4. Word Embedding(워드 임베딩)

* Word2vec
* GloVe
* Fasttext

### 8-5. ETC

* Deep Belief Network
* Deep Dream

## 9. NLP(Natural Language Processing, 자연어 처리)

* Embedding
* Fine-Tuning

## 10. Journal(저널, 학회지), Conference(컨퍼런스)

* ICML(the International Conference for Machine Learning)
* NIPS(Neural Information Processing Systems)
* AAAI(Association for the Advancement of Artificial Intelligence)

## 11. Challenge

* ILSVRC(the ImageNet Large Scale Visual Recognition Challenge)
* [SQuAD](https://rajpurkar.github.io/SQuAD-explorer/)(the Stanford Question Answering Dataset)
* [Alexa Prize Competition](https://developer.amazon.com/alexaprize)

다음 용어들에 대해서는 분류 및 설명 작업이 진행 중입니다.

Affine Layer  
Highway Layer  
Neural Turing Machine  
Noise-contrastive estimation(NCE)  
Restricted Boltzmann Machine(RBN)
