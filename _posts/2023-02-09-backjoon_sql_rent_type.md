---
published: True
title:  "[Programmers, MySQL, Lv.1] 자동차 대여 기록에서 장기/단기 대여 구분하기. 왜 30이 아닌 29?"
categories: CodingTests
tag: [Programmers, MySQL, Database, SQL]
---

## 문제

> CAR_RENTAL_COMPANY_RENTAL_HISTORY 테이블에서 대여 시작일이 2022년 9월에 속하는 대여 기록에 대해서 대여 기간이 30일 이상이면 '장기 대여' 그렇지 않으면 '단기 대여' 로 표시하는 컬럼(컬럼명: RENT_TYPE)을 추가하여 대여기록을 출력하는 SQL문을 작성해주세요. 결과는 대여 기록 ID를 기준으로 내림차순 정렬해주세요.

## 해답

``` SQL
SELECT 
history_id HISTORY_ID, 
car_id CAR_ID, 
DATE_FORMAT(start_date,'%Y-%m-%d') START_DATE, 
DATE_FORMAT(end_date,'%Y-%m-%d') END_DATE , 
CASE WHEN TIMESTAMPDIFF(DAY, START_DATE, END_DATE) >= 29 THEN '장기 대여' ELSE '단기 대여' END RENT_TYPE
FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
WHERE YEAR(START_DATE)='2022' AND MONTH(START_DATE) = '09'
ORDER BY HISTORY_ID desc;
```

## 풀이
문제에서 조건이 ```기간이 30일 이상이면``` 이었기 때문에  
처음에는 아래와 같이 작성했는데 계속 틀렸다고 나왔다.
```
TIMESTAMPDIFF(DAY, START_DATE, END_DATE) >= 30
```

결국 검색해 본 결과 ```>= 30``` 이 아닌 ```>= 29``` 로 해야 한다고 한다.

그런데 ```TIMESTAMPDIFF(DAY, START_DATE, END_DATE)``` 로 일수비교를 해보면  
```2022-09-16``` 과 ```2022-09-16```은 0일 차이로 계산되고  
개인적으로도 1일이 아닌 0일로 봐야 한다고 생각하는데 출제자는 1일로 계산해야 한다고 생각한 것 같다.  
그럴수 있다고 생각은 하지만 
이 부분을 명시해주거나 예시에 있었다면 더 좋았을 것 같다.   
아니면 이 부분이 의도된 함정이었을 수도?   
  