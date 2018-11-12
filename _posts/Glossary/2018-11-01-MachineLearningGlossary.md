---
layout: post
title:  "머신러닝 용어 모음(Machine Learning Glossary)"
date:   2018-11-01 22:00:00
author: 김태희
categories: Glossary
---

머신러닝 알고리즘 용어 모음집을 작성 중입니다.  
여러분들은 아래의 용어에 대해 모두 설명할 수 있으신가요?  
아래의 용어들에 대해 모두 간단하고 쉽게 설명해보는 것을 목표로 공부하면 좋을 것 같습니다 :)  
오타 혹은 오개념에 대해서는 댓글로 남겨주시면 감사하겠습니다.  
지속적인 업데이트가 이루어지고 있습니다.  

피드백은 언제나 환영입니다.

# Methods

## 1. Linear Regression(선형 회귀)

### 1-1. Simple Linear Regression(단순 선형 회귀)

$$ y = \beta x + \varepsilon $$

### 1-2. Multi Linear Regression(중다 회귀, 다중 회귀)

$$ y = { \beta  }_{ 1 }{ x }_{ 1i } + ... + { \beta  }_{ i }{ x }_{ ip } + \varepsilon = { X }_{ i }^{ T }\beta + { \varepsilon  }_{ i } $$

### 1-3. Generalized Linear Model(일반화 선형 모델)

* Concept Note
  1. Multi-colinearity
  2. Ordinary Least Square(OLS)
  3. Maximum Likelihood Estimation(MLE)

## 2. Logistic Regression

$${ h }_{ \theta  }(x)\quad =\quad \frac { 1 }{ 1\quad +\quad { e }^{ -{ \theta  }^{ T }x } }$$

![로지스틱 함수](https://upload.wikimedia.org/wikipedia/commons/thumb/8/88/Logistic-curve.svg/480px-Logistic-curve.svg.png){: width="200" height="300"}

$$-\frac { 1 }{ m } [\sum _{ i=1 }^{ m }{ { y }^{ (i) }log{ h }_{ \theta  }({ x }^{ (i) })+(1-{ y }^{ (i) })log(1-{ h }_{ \theta  }({ x }^{ (i) }))] }$$

## 3. Support Vector Machine(SVM)

![Support Vector Machine](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcS1IQUL_6FPoOQEzmwZBhX2JAhAokXfdJ_9te01U2qE2sNEBoTMAA){: width="200" height="300"}

![Cost function of SVM](http://www.holehouse.org/mlclass/12_Support_Vector_Machines_files/Image%20[12].png)

$$C\sum _{ i=1 }^{ m }{ [{ y }^{ (i) }{ cost }_{ 1 }({ \theta  }^{ T }{ x }^{ (i) })+(1-{ y }^{ (i) }{ cost }_{ 0 }{ \theta  }^{ T }{ x }^{ (i) })]\quad +\quad \frac { 1 }{ 2 } \sum _{ i=1 }^{ n }{ { \theta  }_{ j }^{ 2 } }  } $$

### 3-1. Soft Margin SVM

### 3-2. SVM with Kernel Trick

$${ \theta  }_{ 1 }{ f }_{ 1 }+{ \theta  }_{ 2 }{ f }_{ 2 }+...+{ \theta  }_{ n }{ f }_{ n }$$

$${ f }_{ i } = similarity(x,{ l }^{ (i) }) = exp(\frac { \left\| x-{ l }^{ (i) } \right\| ^{ 2 } }{ 2{ \sigma  }^{ 2 } } )$$

## 4. Decision Tree(의사결정 나무)

## 5. Random Forest

## 6. K-means Clustering

## 7. Latent Semantic Analysis(LSA)

* Concept Note
 - Singular Value Decomposition(SVD)

## 8. Latent Dirichlet Allocation(LDA)

* Concept Note
 - Dirichlet Distribution
 - truncked SVD
## 9. Naive Bayes

## 10. Boosting

### 10-1. xgboost
### 10-2. LightGBM
### 10-3. Catboost

* Concpet Note
 - Ensemble

## 11. Bagging

* Concept Note
 - Ensemble

## 13. Ensemble(앙상블)

## 14. Bias-Variance Trade Off

# Dimension Reduction(차원 축소)

## 1. Principal Component Analysis(PCA)

* Concept Note
 - Singular Value Decomposition
 - Eigen Value, Eigen Vector

## 2. t-Distributed Stochastic Neighbor Embedding(t-SNE)

* Concept Note
 - Student's t-distribution(t-distribution, gaussian distribution)

## 3. Auto-Encoder

# Technic

## 1. Cross Validation

## 2. Regularization

### 2-1. Ridge

### 2-2. Rasso

# Metric

## 1. RMSE(Root Mean Square Error)

## 2. MSE(Mean Square Error)

## 3. Accuracy

## 4. Precision

## 5. Recall(Sensitivity, TP Rate, Hit Rate)

## 6. F-1 score

## 7. ROC curve

###### etc
- Markov Chain
- Association Rule
  - Support
  - Confidence
  - Lift
