---
layout: post
title:  "Random Search vs Grid Search"
date:   2019-01-30 21:00:00
author: entiff
categories: Python
---

이번 글에서는 hyper-parameter 최적화에 대해 알아봅니다. 일반적인 parameter(weight)는 학습 과정에서 조정됩니다. 그러나 hyper-parameter(ex. Learning rate, Batch size 등)는 고정된 값으로 학습되며, 학습 이전에 우리가 지정해야 합니다. 데이터 셋마다 최적의 hyper-parameter가 다르고, 더 좋은 모델도 hyper-parameter 최적화 없이는 안좋은 결과를 얻기 때문에 hyper-parameter 최적화는 매우 중요합니다.

### Hyper-parameter Optimization

대표적인 hyper-parameter 최적화 방법은 Manual Search, Grid Search, Random Search, Bayesian Search가 있습니다. Manual Search는 말그대로 연구자 임의대로 hyper-parameter를 찾는 방법입니다. Grid Search 역시 크게 다르지 않습니다. Grid Search는 hyper-parameter의 범위와 간격을 미리 정해 각 경우의 수에 모두 대입하여 최적의 경우의 수를 찾습니다. hyper-parameter 4개가 있고 각각의 경우의 수가 10개이면, `10 x 10 x 10 x 10`만큼의 경우의 수를 탐색합니다. Random Search는 정해진 범위에서 난수를 생성해 최적의 hyper-parameter를 찾습니다. Bayesian Search는 surrogate 모델을 따로 설정하고 이 모델의 parameter를 업데이트하며 hyper-parameter를 찾습니다. surrogate 모델 역시 hyper-parameter를 갖고 있습니다. 이번 글에서는 Grid Search와 Random Search를 비교합니다.

### Grid Search vs Random Search

[Random Search for Hyper-Parameter Optimization(J Bergstra, 2012)](http://www.jmlr.org/papers/volume13/bergstra12a/bergstra12a.pdf)에 따르면 Random Search가 여러가지 면에서 유리합니다. 먼저 Random Search는 중요한 hyper-parameter를 더 많이 탐색합니다. 다음의 그림을 보겠습니다.

![img](https://github.com/shwksl101/shwksl101.github.io/blob/master/images/randomvsgird.PNG?raw=true)

Grid Search는 중요한 hyper-parameter의 3개 지점을 탐색했습니다. 반면, Random Search는 9개 지점을 탐색했습니다. 이처럼, hyper-parameter는 상대적 중요도가 서로 다르고 Random Search는 중요한 parameter를 더 많이 탐색할 수 있기 때문에 optimization하기에 유리합니다. 반면, Grid Search는 중요하지 않은 hyper-parameter를 너무 많이 탐색한다고 합니다.

실제로 논문에서는 Random Search가 더 적은 탐색 기회로 더 좋은 결과를 냈습니다.

![img](https://github.com/shwksl101/shwksl101.github.io/blob/master/images/gridvsrandom.PNG?raw=true)

파란색 점선은 Grid Search(100번) 결과이고 까만색 점선은 Random Search(64번) 결과입니다. Random Search와 같은 결과를 내기 위해서 grid search는 약 500번 정도 수행해야 했다고 합니다.

다음으로 Random Search는 언제든지 탐색을 중단할 수 있습니다. 중간에 멈추더라도 특정 범위에 편중된 탐색이 아니기 때문입니다. 그러나 Grid Search는 탐색을 중단하면 hyper-parameter의 일부 범위를 제외하고 탐색합니다.

그러나, 여전히 Grid Search는 많이 쓰이고 guideline을 개선하는 방식으로 사용되고 있는 것 같습니다.

대중적인 guideline은 다음과 같습니다.

1. 경험적으로 중요한 hyper-parameter를 먼저 탐색하고 값을 고정합니다.
2. 덜 중요한 hyper-parameter를 나중에 탐색합니다.  
3. 먼저 넓은 범위에 대해 hyper-parameter를 탐색하고 좋은 결과가 나온 범위에서 다시 탐색합니다.

### Conclusion

위의 guideline은 Random Search에도 충분히 적용할 수 있어 앞으로의 연구에서는 Random Search가 더 많이 사용되지 않을까 생각합니다. Bayesian optimization 역시 impractical하다는 평이 있지만 [Practical bayesian optimization of machine learning](https://arxiv.org/pdf/1206.2944.pdf)과 같은 논문의 등장으로 Bayesian 기반의 optimization도 더 많이 사용될 것 같습니다.
