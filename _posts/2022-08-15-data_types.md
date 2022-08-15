---
published: false
title:  "정형 데이터의 종류"
categories: Data
tag: [data, statistics, 통계]
---

"데이터과학을 위한 통계" 책을 읽고 있다.  
앞으로 거기에 나오는 개념들을 포스팅해보려 한다.  


## 데이터의 종류를 구분하는 이유
- 데이터 분석, 예측 모델링 할 때, 데이터 종류에 따라 시각화, 해석, 통계 모델 결정 방법이 달라진다.  
- 데이터를 다루는 프로그램에서 데이터 종류를 어떻게 나누는가에 따라  
    연산속도의 차이가 있기 때문에 잘 분류하는 것이 좋다.  


## 정형 데이터의 종류

보통 RDBMS에 들어가는 정형화된 데이터 종류.  


## 수치형 (Numeric)
- 숫자를 이용해 표현 할 수 있는데이터
- 연속형 데이터, 이산 데이터 등이 포함된다.

    ### 연속형 (Continuous)
    - 일정 범위 안에서 어떤 값이든 취할수 있는 데이터
    - 실수로 표현 가능
    - 평균과 표준편차, 분산으로 표현 가능
    - 예) 나이, 몸무게

    ### 이산 (Discrete)
    - 횟수와 같이 정수값만 취할 수 있는 데이터
    - 예) 불량품 수, 무단횡단 횟수



## 범주형 (Categorical), 다항형 (Polychotomous)
- 가능한 범주 안의 값만을 취하는 데이터
- 명목형 데이터, 순서형 데이터 등이 포함됨 

    ### 명목형 (Nomial)
    - 순서가 없는 범주형 데이터
    - 예) 좋아하는 음식, 혈액형, 성공여부

    ### 순서형 (Ordinal)
    - 값들 사이에 순서가 있는 범주형 데이터
    - 예) 설문조사결과(나쁨,보통,중간), 영화 별점(1~5개)

    ### 이진 (Binary)
    - 두 개의 값 만을 갖는 범주형 데이터
    - 예) 참/거짓, 생존/사망

