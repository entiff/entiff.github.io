---
layout: post
title:  "L1, L2 정규화(regularization) 비교"
date:   2019-01-27 21:00:00
author: entiff
categories: Python
---

Machine Learning에서 과적합(Overfitting) 문제를 해결하는 방법에는 여러가지가 있습니다.

첫번째는 모델이 훈련하는 데이터의 양을 늘리는 것입니다. 가장 확실한 방법이지만 시간과 비용이 많이 든다는 단점이 있습니다.
두번째는 모델의 feature 개수를 줄이는 것입니다. model selection 알고리즘을 이용하거나 종속변수와 상관이 낮은 feature를 직접 제거할 수도 있습니다.
세번째는 Regularization입니다. 최소화하고자 하는 Loss function에 Regularization term을 더합니다. 이때의 Regularization term은 L1, L2, Elastic net 정도가 있습니다.

이번 글에서는 L1과 L2 regularization의 차이를 gradient 관점에서 해석해보고자 합니다.

## Linear Regression with Gradient Descent

선형회귀식의 Loss function은 다음과 같습니다.

$$ J(\theta )=\frac { 1 }{ 2m } \left[ \sum _{ i=1 }^{ m }{ ({ h }_{ \theta  }({ x }^{ (i) })-{ y }^{ (i) }{ ) }^{ 2 } }  \right]  $$

이를 바탕으로 theta는 다음과 같이 업데이트됩니다.

$$ { \theta  }_{ j }:={ \theta  }_{ j }-\alpha \quad \left[ { \frac { 1 }{ m }  }\sum _{ i=1 }^{ m }{ ({ h }_{ \theta  }({ x }^{ (i) })-{ y }^{ (i) }{ ) }{ x }_{ j }^{ (i) } }  \right] $$

Python으로 작성하면 다음과 같습니다.

```
# y = theta1*x1+theta2*x2+theta3*x3+theta4*x4+theta5*x5
def Gradientdescent(X, y, theta_start):
    theta = theta_start

    for meaningless in range(iterations):

        error = np.zeros(5)
        error = (1/len(X))*np.sum((h(theta,X)-y).reshape(-1,1)*X,axis=0)
        theta = theta - learningrate*error
```

## Linear Regression with Gradient Descent + L1 regularization

기존 Loss에 L1 regularization term을 추가하면 다음과 같습니다.

$$ J(\theta )=\frac { 1 }{ 2m } \left[ \sum _{ i=1 }^{ m }{ ({ h }_{ \theta  }({ x }^{ (i) })-{ y }^{ (i) }{ ) }^{ 2 } }  \right] +\lambda \sum _{ j=1 }^{ n }{ { \left| { \theta  }_{ j } \right|  } } $$

reg term을 2m으로 나누어 대괄호 안에 들어가더라도 상관없습니다. 다만 그 경우에 최적의 람다값이 2m배 차이나게 됩니다.

새로 정의된 loss로 gradient descent를 수행하면 다음과 같습니다.

```
# y = theta1*x1+theta2*x2+theta3*x3+theta4*x4+theta5*x5
def Gradientdescent(X, y, theta_start):
    theta = theta_start

    for meaningless in range(iterations):
        error = np.zeros(5)

        for i in range(len(theta)):
            if theta[i] > 0:
                error[i] = (1/len(X))*np.sum((h(theta,X)-y)*X[:,i]) + lambda * 1
            else:
                error[i] = (1/len(X))*np.sum((h(theta,X)-y)*X[:,i]) - lambda * 1

        theta = theta - learningrate*error
```

theta가 0보다 큰 경우 reg term은 `lamba`가 되고 0보다 작은 경우에는 `-lambda`가 됩니다. 이러한 성질은 `y= |x|` 그래프의 미분을 생각해보면 간단합니다.

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/6/6b/Absolute_value.svg/330px-Absolute_value.svg.png){: width="300" height="300"}

0보다 클 때의 기울기는 1, 작을 때의 기울기는 0입니다.

## Linear Regression with Gradient Descent + L2 regularization

이번에는 L2 reg term을 추가해보겠습니다. 새롭게 정의된 Loss function은 다음과 같습니다.

$$ J(\theta )=\frac { 1 }{ 2m } \left[ \sum _{ i=1 }^{ m }{ ({ h }_{ \theta  }({ x }^{ (i) })-{ y }^{ (i) }{ ) }^{ 2 } }  \right] +\lambda \sum _{ j=1 }^{ n }{ { \theta  }_{ j }^{ 2 } } $$

이 경우 theta는 다음과 같이 업데이트 됩니다.

$$ { \theta  }_{ 0 } := { \theta  }_{ 0 }-\alpha \frac { 1 }{ m } \sum _{ i=1 }^{ m }{ ({ h }_{ \theta  }({ x }^{ (i) })-{ y }^{ (i) }) } { x }_{ 0 }^{ (i) }\quad $$

$$ { \theta  }_{ j } := { \theta  }_{ j }-\alpha \frac { 1 }{ m } \sum _{ i=1 }^{ m }{ ({ h }_{ \theta  }({ x }^{ (i) })-{ y }^{ (i) }) } { x }_{ j }^{ (i) }-\quad 2\lambda { \theta  }_{ j } $$

reg term을 2m으로 나누어 대괄호 안에 들어가더라도 상관없습니다. 다만 그 경우에 최적의 람다값이 4m배 차이나게 됩니다.

```
# y = theta1*x1+theta2*x2+theta3*x3+theta4*x4+theta5*x5
def Gradientdescent(X, y, theta_start):
    theta = theta_start

    for meaningless in range(iterations):

        error = np.zeros(5)
        error = (1/len(X))*np.sum((h(theta,X)-y).reshape(-1,1)*X,axis=0) + 2*lambda*theta
        theta = theta - learningrate*error
```

L2 reg term에서는 theta의 크기에 상관없이 `2 x lambda x theta`가 loss에 추가됩니다.

Gradient 관점에서 L1과 L2를 비교하면, L1은 theta의 부호에 따라 lambda를 더하거나 빼주고
L2는 해당 theta에 `2 x lambda`가 곱해져서 더해집니다. 이에 따라 L1은 theta에 일정한 크기(lambda)로 영향을 주고
L2는 theta가 작아지면 조금 더해지고 theta가 클수록 많이 더해집니다. 이를 시각화 하면 다음과 같습니다.

![L1L2](https://github.com/shwksl101/shwksl101.github.io/blob/master/images/L1L2.png?raw=true)

L1과 L2 모두 regularization 효과로 theta를 0으로 보내고자 합니다. 그러나, L1의 경우 theta의 크기에 상관없이
일정한 힘으로 0 방향으로 theta를 이동시킵니다. 반면, L2의 경우 theta의 크기가 작아질수록 0으로 theta를 이동시키는 힘이
약해집니다. 이러한 차이로 L1은 많은 feature들의 가중치를 0으로 만들고 L2는 0으로 만들지 않고 0보다 아주 가까운 값으로 만듭니다.

다른 그림으로도 한번 보겠습니다.

L1의 가중치 변화입니다.

![L1](https://github.com/shwksl101/shwksl101.github.io/blob/master/images/L1%20Norm.png?raw=true)

람다(여기서는 L1 Norm)의 변화에도 대부분의 coefficient 즉, theta가 0임을 알 수 있습니다.

L2의 가중치 변화입니다.

![L2](https://github.com/shwksl101/shwksl101.github.io/blob/master/images/L2%20Norm.png?raw=true)

가중치들이 0에 가깝지만 대부분 0이 아닌 값을 갖고 있습니다.

이처럼 L1과 L2는 서로 다른 효과를 나타냅니다. 일반적으로 L1은 sparse solution을 유도합니다. 일부의 feature만으로 solution을 찾는 것입니다. 반대로 L2는 영향력이 낮은 feature를 유지하고 이를 통해 더 좋은 solution을 찾고자 합니다.

#### Reference
  - 고려대 주재걸 교수님 [강의](https://www.youtube.com/watch?v=W-93giPwZnk&list=PLep-kTP3NkcNQ6kVMzAYhp3atuo2kCp2B&index=5)
