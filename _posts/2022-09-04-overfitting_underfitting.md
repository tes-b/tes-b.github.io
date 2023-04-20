---
published: true
title:  "과적합(Overfitting)과 과소적합(Underfitting)"
categories: [DataScience]
tag: [data, DataScience, MachineLearning, overfitting, underfitting]
---

테스트 데이터에서 만들어내는 오차를 일반화 오차라고 한다.  
훈련데이터와 테스트 데이터 모두에서 좋은 성능을 내는 모델은 일반화가 잘된 모델이다.  
일반화 오차가 큰 모델은 두가지 이유가 있는데 과적합과 과소적합이다.

## 과적합(Overfitting)
- 훈련 데이터셋의 디테일과 노이즈까지 모두 학습하여  
새로운 데이터를 잘 예측하지 못하는 현상이다.

- 과적합 된 모델은 훈련 데이터셋에서의 성능은 높지만  
테스트 데이터셋에서의 성능은 좋지 못하다.

- 유연성이 높은 non-parametric모델이나 비선형 모델에서 나타날 가능성이 높다.  

## 과소적합(Underfitting)
- 과소적합은 훈련 데이터셋도 제대로 학습하지 못해서  
새로운 데이터를 잘 예측하지 못하는 현상을 말한다.  

- 과소적합 된 모델은 훈련 데이터셋과 테스트 데이터셋  
모두에게서 성능이 낮다.