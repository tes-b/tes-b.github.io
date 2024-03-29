---
published: false
title:  "정규화 회귀모델 - 릿지 회귀 (Ridge Regression)"
categories: [datascience]
tag: [data, data science, machine-learning, ridge]
---

정규화 회귀모델은 선형모델에 규제항을 더해 과적합을 방지하는 방법이다.  
규제항은 회귀계수를 감소시켜 예측에 미치는 영향력을 축소시킨다.  

분산과 편향 트레이드오프(Trade-off) 관점에서 보면  
모델에 편향을 조금 더하고 분산을 줄여서 좀 더 일반화된 모델을 만드는 기법이다.  


정규화 회귀모델은 규제항에 따라 3가지로 구분된다.  

- Ridge : L2 Penalty(가중치들의 제곱합)
- Lasso : L1 Penalty(가중치들의 절댓값 합)
- ElasticNet : L1 Penalty + L2 Penalty

정규화 회귀모델은 입력 특성의 스케일에 민감하기 때문에  
반드시 특성의 스케일을 표준화 해줘야 한다.  


## 릿지회귀(Ridge Regression)
릿지회귀는 회귀계수에 가중치들의 제곱합을 패널티로 부과하여 회귀계수를 줄이는 모델이다.  
L2 패널티를 적용하면 영향력이 크지 않은 회귀계수의 값은 0에 가까운 수로 축소한다.  
릿지회귀의 비용함수는 다음과 같다.

$$
cost=RSS+ \lambda\sum^p_{j=1}\beta^2_{j}=\sum^n_{i=1}(y_i - \beta_0 -\beta_1x_{i1}-...-\beta_px_{ip})^2+\lambda\sum^p_{j=1}|\beta^2_j \\
RSS\,(Residual\,Sum\,of\,Squares)
$$


비용함수
원래의 값과 가장 오차가 작은 가설함수를 도출하기 위해 사용되는 함수이다.  

가설함수의 형태를 결정짓는 것은 매개변수(parameter, θ)이다.  
이 값을 적절하게 조정해 실제값 y에 가장 근접한 가설함수를 트레이닝 셋을 이용해 도출해야 한다.  
$$
J(θ_0,θ_1)={1\over2m}\sum^m_{i=1}(h_θ(x^{(i)}-y^{(i)})^2\\
J(θ_0,θ_1)={1\over2m}\sum^m_{i=1}(잔차)^2\\
잔차(Residual) = 예측값과\,실제값의\,차이
$$
$h(x)-θ$은 '가설함수와 실제 y값의 차이'입니다. 이것을 최소화하는 것이 목표이지만 그냥 사용하면 오차가 양수 혹은 음수가 될 수 있으므로 제곱을 해줍니다. 그리고 Training set은 1부터 m까지 존재하기 때문에 각각의 차이를 모두 더하여 평균을 내어 평균이 최소가 되게 만드는 θ를 구하는 것이 비용함수의 목적이다. 이때 m이 아닌 2m으로 나누는 이유는 미분했을 때 내려오는 2를 자연스럽게 나눠지게 하기 위해서이다.  


$$
MSE = \frac{1}{N}\sum_{i=1}^{N}(y_{i}-\widehat{y}_{i})^2\
$$

$$ RSS = \sum_{i=1}^{N} {\hat{e_i}}^2 =  \sum_{i=1}^{N} {(y_i - (\hat{\alpha}x_i + \hat{\beta}))}^2 
$$

$ \hat{\alpha} = \frac{S_{xy}}{S_{xx}} $  

$ \hat{\beta} = \bar{y} - \hat{\alpha}\bar{x}$


$ S_{xy}= {\sum_{i=1}^{N}(x_i-\bar{x})(y_i-\bar{y})}$

$ S_{xx} = {\sum_{i=1}^{N}(x_i-\bar{x})^2}$
