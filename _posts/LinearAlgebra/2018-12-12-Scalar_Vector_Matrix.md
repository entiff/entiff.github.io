---
layout: post
title:  "Linear Algebra(선형대수학) 기초"
date:   2018-12-12 19:00:00
author: entiff
categories: LinearAlgebra
---

## Sclar(스칼라)

Scalar(스칼라)는 크기만 있고 방향을 가지지 않는 양입니다. 좀 더 정확한 정의는 n차원 공간에서 n의 0승개의 숫자(1개)로 표현할 수 있는 물리량입니다. 따라서, 좌표계가 변해도 양은 변하지 않습니다. 간단하게 말하자면 하나의 숫자입니다.

## Vector(벡터)

대조적으로 Vector(벡터)는 크기와 방향이 모두 있습니다. 선형대수학에서는 이를 `nx1`행렬 또는 `1xn`행렬로 나타내고 각각을 열벡터와 행벡터로 부르기도 합니다. 방향을 나타내기 때문에 벡터 표현에 화살표를 이용할 수 있습니다.

$$x\quad =\quad \begin{pmatrix} x_{ 1 } \\ x_{ 2 } \\ \vdots  \\ x_{ n } \end{pmatrix}\quad \in \quad { R }^{ n }$$

### 벡터의 Inner product(내적)

두 벡터를 연산해 하나의 스칼라에 대응시키는 함수를 내적(inner product)이라 합니다.

$$X\quad =\quad \begin{pmatrix} x_{ 1 } \\ x_{ 2 } \\ \vdots  \\ x_{ n } \end{pmatrix}\quad Y\quad =\quad \begin{pmatrix} y_{ 1 } \\ y_{ 2 } \\ \vdots  \\ y_{ n } \end{pmatrix}$$

$$<X,Y>\quad =\quad { x }^{ T }y\quad =\quad \sum _{ i=1 }^{ n } x_{ 1 }y_{ 1 }+x_{ 2 }y_{ 2 }+\cdots +x_{ n }y_{ n }$$

### 벡터의 길이

X벡터를 다음과 같이 정의하고 길이를 구하고자 합니다.

$$X\quad =\quad \begin{pmatrix} x_{ 1 } \\ x_{ 2 } \\ \vdots  \\ x_{ n } \end{pmatrix}\quad y\quad =\quad \begin{pmatrix} y_{ 1 } \\ y_{ 2 } \\ \vdots  \\ y_{ n } \end{pmatrix}$$

x벡터의 길이(Lx)는 각 요소의 제곱에 제곱근을 씌워서 구합니다.

$$Lx\quad =\quad \sqrt { { x }_{ 1 }^{ 2 }+x_{ 2 }^{ 2 }+...+x_{ n }^{ 2 } } \quad =\quad \sqrt { { X }^{ T }X }$$

### 벡터 간 각도

벡터 X와 Y 사이의 각도는 다음과 같습니다. 두 벡터의 내적값을 각 벡터의 길이로 나눠준 값입니다.

$$cos(\theta )\quad =\quad \frac { (x_{ 1 }y_{ 1 }+x_{ 2 }y_{ 2 }+...+x_{ n }y_{ n }) }{ Lx\cdot Ly } =\frac { { x }^{ T }y }{ \sqrt { x^{ T }x } \cdot \sqrt { y^{ T }y }  }$$

## Matrix(행렬)

Matrix(행렬)은 row(행)와 column(열)로 나타냅니다.

$$A\quad =\quad \begin{pmatrix} 1 & 2 \\ 3 & 4 \\ 5 & 6 \end{pmatrix}\in \quad { R }^{ 3x2 }$$

이때, {1,2}를 1행, {3,4}를 2행, {5,6}을 3행이라 합니다.
{1,3,5}는 1열, {2,4,6}은 2열입니다.

여기서 중요한 것은 행렬을 열벡터 기준으로 읽는다는 것입니다.
위 행렬의 경우 행의 개수인 3이 dimension(차원)이 되고, 열의 개수인 2가 벡터의 개수가 됩니다.

행벡터는 주로 transpose(전치)를 이용해 표현합니다.

$$x^{ T }\quad =\quad \begin{pmatrix} x_{ 1 } \\ x_{ 2 } \\ \vdots  \\ x_{ n } \end{pmatrix}\quad =\quad \begin{pmatrix} x_{ 1 } & x_{ 2 } & \cdots  & x_{ n } \end{pmatrix}\in \quad { R }^{ 1xn }$$

행과 열의 개수가 같은 경우 Square matrix(정방행렬), 다른 경우 Rectangular matrix(직사각행렬)라 합니다.

$${ A }_{ ij }$$로 행렬의 요소에 접근합니다. 여기서 $${ A }_{ 2,1 }$$은 3입니다.

### 행렬의 덧셈

행렬의 덧셈은 더하는 행렬의 크기가 같아야 가능합니다. `C = A+B`라 할 때, A행렬이 `2x3`행렬이라면 B행렬도 `2x3` 행렬이어야 연산이 가능합니다.

$$A\quad =\quad \begin{pmatrix} 1 & 2 \\ 3 & 4 \\ 5 & 6 \end{pmatrix}\quad B\quad =\quad \begin{pmatrix} 1 & 2 \\ 3 & 4 \\ 5 & 6 \end{pmatrix}\quad \\ \\ A+B\quad =\quad \begin{pmatrix} 2 & 4 \\ 6 & 8 \\ 10 & 12 \end{pmatrix}$$

### 행렬의 곱셈

행렬은 스칼라나 행렬과 곱해질 수 있습니다. 사실 스칼라는 스칼라, 벡터, 행렬 모두와 곱해질 수 있습니다.

A행렬에 a스칼라를 곱하면 다음과 같습니다.

$$a\quad =\quad 2,\quad A\quad =\quad \begin{pmatrix} 1 & 2 \\ 3 & 4 \\ 5 & 6 \end{pmatrix}$$

$$aA\quad =\quad 2\begin{pmatrix} 1 & 2 \\ 3 & 4 \\ 5 & 6 \end{pmatrix}\quad =\quad \begin{pmatrix} 2 & 4 \\ 6 & 8 \\ 10 & 12 \end{pmatrix}$$

행렬간의 곱셈은 곱해지는 왼쪽 행렬의 열과 오른쪽 행렬의 행 개수가 같아야 합니다.

$$A\quad =\quad \begin{pmatrix} 1 & 2 & 3 \\ 4 & 5 & 6 \end{pmatrix}\quad B\quad =\quad \begin{pmatrix} 1 & 2 \\ 3 & 4 \\ 5 & 6 \end{pmatrix}$$

$$A\cdot B\quad =\quad \begin{pmatrix} A_{ 1 }_{ 1 }\cdot B_{ 1 }_{ 1 }+A_{ 1 }_{ 2 }\cdot B_{ 2 }_{ 1 }+A_{ 1 }_{ 3 }\cdot B_{ 3 }_{ 1 }\quad  & A_{ 1 }_{ 1 }\cdot B_{ 1 }_{ 2 }+A_{ 1 }_{ 2 }\cdot B_{ 2 }_{ 2 }+A_{ 1 }_{ 3 }\cdot B_{ 3 }_{ 2 } \\ A_{ 2 }_{ 1 }\cdot B_{ 1 }_{ 1 }+A_{ 2 }_{ 2 }\cdot B_{ 2 }_{ 1 }+A_{ 2 }_{ 3 }\cdot B_{ 3 }_{ 1 }\quad  & A_{ 2 }_{ 1 }\cdot B_{ 1 }_{ 2 }+A_{ 2 }_{ 2 }\cdot B_{ 2 }_{ 2 }+A_{ 2 }_{ 3 }\cdot B_{ 3 }_{ 2 } \end{pmatrix}$$

이 경우 BA는 성립하지 않습니다.
