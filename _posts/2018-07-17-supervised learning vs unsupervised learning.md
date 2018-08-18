---
layout: post
title:  "Supervised learning vs Unsupervised learning"
date:   2017-07-17 08:00:00
author: shwksl101
categories: Machine_Learning
---

지도학습과 비지도학습의 정의, 그리고 그 차이에 대해서 알아보고자 합니다.  
[앤드류응](https://www.coursera.org/learn/machine-learning/lecture/1VkCb/supervised-learning) 교수님의 강의를 정리한 글임을 밝힙니다.  

지도학습은 알고리즘에게 정확한 답을 알고 있는 데이터 셋을 줍니다.  

![default](https://user-images.githubusercontent.com/25002275/44298799-b07e0880-a324-11e8-931b-3b3224b00ce5.PNG "주택가격"){: width="500" height="300"}

위 그림은 주택 가격 예측 그래프입니다. 여기서 지도 학습은 학습 알고리즘에 실제 데이터를 알려줍니다.  
다시 말해, 주택 넓이에 대한 가격의 예시 데이터를 알고리즘에게 알려주는 것입니다.  
그리고 다시 주어진 조건에 대해 알맞은 답을 찾도록 합니다.

이러한 문제는 regression(회귀) 문제로 볼 수 있습니다. 이산적으로 주어진 데이터에서 연속적인 값을 예측하고자 하기 때문입니다.  

![default](https://user-images.githubusercontent.com/25002275/44298756-cc34df00-a323-11e8-80f6-e48c5520c156.PNG "악성종양"){: width="500" height="300"}

위 그림은 유방의 종양 크기에 따라 악성 종양 여부를 나타낸 그림입니다. 파란색은 양성 종양, 빨간색은 악성 종양입니다.  

여기서의 질문은 다음과 같습니다.  
새로운 종양을 발견하고 그 크기를 알 때 악성 종양일 확률은 얼마나 되는가? 종양이 악성인지 아닌지 판단하고자 하는 것입니다.  
이것은 classification(분류) 문제로 정의할 수 있습니다.  

위 사례 역시 종양의 크기를 바탕으로 한 악성 종양 여부 데이터를 알고리즘이 학습합니다.  
그리고 학습한 알고리즘은 새로운 자료에 대해 종양이 악성인지 아닌지 판단하게 됩니다.  

유방암 classification 문제를 조금 더 확장해보겠습니다. 단순히 종양을 악성과 양성으로만 나누는 것이 아니라 악성 종양에서도 유형을 분류할 수 있습니다.  
또 다른 한편으로는 종양의 종류를 결정하는 데에 사용되는 feature가 크기 뿐만 아니라 다른 여러 feature들이 사용될 수 있습니다.  

이렇듯 classification 종류가 많아지거나 사용되는 feature가 계속 늘어나면 어떻게 될까요?  
이러한 문제를 컴퓨터가 풀기 위해서는 고도의 알고리즘이 필요하게 됩니다. 이러한 알고리즘 종류 중의 하나가 Support Vector Machine입니다.  
SVM(Support Vector Machine)에 대한 내용은 차후에 다루도록 하겠습니다.  

정리하자면, supervised learning에서는 주어진 데이터 셋이 있고 그것들을 바탕으로 input과 output의 관계를 찾아냅니다.  
그리고 supervised learning은 regression와 classification 문제로 나뉩니다. regression에서는 input variables를 continuous function에 mapping합니다.  
classification에서는 input variables를 discrete categories에 mapping합니다.  

다음은 비지도학습입니다.  

앞선 예시를 들어 supervised learning과 unsupervised learning을 비교해보겠습니다.  

![default](https://user-images.githubusercontent.com/25002275/44298758-d7880a80-a323-11e8-9082-8b41d752f422.PNG "supervised learning"){: width="300" height="300"}
![default](https://user-images.githubusercontent.com/25002275/44298759-e078dc00-a323-11e8-8174-c72f12186f5c.PNG "unsupervised learning"){: width="300" height="300"}

supervised learning에서는 위와 같이 어느 데이터가 악성 종양인지 양성 종양인지를 알려줍니다.  
반면 unsupervised learning에서는 악성인지 양성인지에 대한 정보를 제공하지 않습니다.  
주어진 데이터 셋에서 구조를 알아서 찾아내는 것입니다.  

clustering 문제가 이에 해당합니다. 서로 자주 이메일을 보내는 친구가 누구인지, 어떤 사람들이 주로 A 상품을 구매하는지 등에 대해 우리는 정답을 모릅니다.  
다만 엄청난 크기의 데이터만을 가지고 있습니다. 이러한 데이터 셋을 이용해 알아서 구조를 찾아내도록 하는 것이 unsupervised learning의 대표적인 예시입니다.  
