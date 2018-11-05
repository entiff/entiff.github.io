---
layout: post
title:  "SVM(Support Vector Machine, 서포트 벡터 머신)"
date:   2018-11-05 15:00:00
author: 김태희
categories: MachineLearning
---

SVM 역시 Classification 문제에 자주 쓰입니다. SVM은 Margin(여백)을 최대화하여
일반화를 꾀하는 알고리즘입니다.

![Support Vector Machine](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcS1IQUL_6FPoOQEzmwZBhX2JAhAokXfdJ_9te01U2qE2sNEBoTMAA)

위의 그림을 보면 SVM은 Margin이 가장 큰 Hyperplane(초평면)을 찾아내는 것이 목적입니다.

Hyper plane은 2차원 공간의 경우 ${ w }_{ 1 }{ x }_{ 1 }{ +w }_{ 2 }{ x }_{ 2 }=b$
3차원 공간의 경우 ${ w }_{ 1 }{ x }_{ 1 }{ +w }_{ 2 }{ x }_{ 2 }+{ w }_{ 3 }{ x }_{ 3 }=b$
이렇게 나타냅니다.
Vetor representation(벡터 표현)으로는 ${ W }^{ T }X+b=0$ 이렇게 나타냅니다.

Margin을 최대화 하는 Hyperplane은 다음과 같이 결정합니다.
Hyperplane으로부터 가장 가까운 sample과의 거리를 측정하고, 이 거리의 2배를 Margin으로 정의합니다.
이때 가장 가까운 sample을 바로 support vector라고 부릅니다.

이를 수학적으로 나타내면 $H1: { W }^{ T }X+b=1$ $H2: { W }^{ T }X+b=-1$로 두 개의
임의의 hyper plane을 가정하고 이 사이의 거리를 측정합니다.
1과 -1은 H1과 H2 사이의 최소 거리를 두기 위해 설정되었습니다.
이는 다음과 같이 정리됩니다. $\frac { 2 }{ \left\| W \right\|  }$

이제 $\frac { 2 }{ \left\| W \right\|  }$를 최대화하면 됩니다.

$max \frac { 2 }{ \left\| W \right\|  }$ 는 $min\quad \frac { { W }^{ T }W }{ 2 }$ 과 같습니다.

제약식은 다음과 같습니다.
${ y }_{ i }=1$일때, $H1: { W }^{ T }X_{ i }+b \ge 1$
${ y }_{ i }=-1$일때, $H2: { W }^{ T }X_{ i }+b \le -1$

이를 한번에 묶어서 표현하면 다음과 같습니다.

${ y }_{ i }({ W }^{ T }{ X }_{ i }+b)-1\ge 0$

이제 위의 목적식($min\quad \frac { { W }^{ T }W }{ 2 }$)과 제약식(${ y }_{ i }({ W }^{ T }{ X }_{ i }+b)-1\ge 0$)을 라그랑지안 승수법과 KKT 조건을 이용하면 문제는 다음과 같이 변환됩니다.

$$Max\quad L\quad =\quad \sum _{ j=1 }^{ N }{ { \alpha  }_{ j } } -\frac { 1 }{ 2 } \sum _{ i=1 }^{ N }{ \sum _{ j=1 }^{ N }{ { \alpha  }_{ i }{ \alpha  }_{ j }{ y }_{ i }{ y }_{ j }{ X }_{ i }^{ T }{ X }_{ j } }  }$$

$$ \sum _{ j=1 }^{ N }{ { \alpha  }_{ j }{ y }_{ j }=0 } $$

결국 SVM은 조건을 만족하는 최적 해를 찾고 w를 찾은 다음 b를 찾습니다.
학습된 $H:{ W }^{ T }X+b=0$에 대해 새로운 test X 값을 넣고 0보다 크면 H1 그룹, 작으면 H2 그룹으로 분류합니다.