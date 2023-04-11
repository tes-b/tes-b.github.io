---
published: True
title:  "[Programmers, MySQL, Lv.4] 자동차 대여 기록 별 대여 금액 구하기"
categories: CodingTests
tag: [Programmers, MySQL, Database, SQL]
---

## 문제

> CAR_RENTAL_COMPANY_CAR 테이블과 CAR_RENTAL_COMPANY_RENTAL_HISTORY 테이블과 CAR_RENTAL_COMPANY_DISCOUNT_PLAN 테이블에서 자동차 종류가 '트럭'인 자동차의 대여 기록에 대해서 대여 기록 별로 대여 금액(컬럼명: FEE)을 구하여 대여 기록 ID와 대여 금액 리스트를 출력하는 SQL문을 작성해주세요. 결과는 대여 금액을 기준으로 내림차순 정렬하고, 대여 금액이 같은 경우 대여 기록 ID를 기준으로 내림차순 정렬해주세요.

## 해답

```sql
SELECT history_id, 
       FLOOR(IF (duration >= 7, 
                 (duration * daily_fee) * (SELECT (100-discount_rate) * 0.01
                                           FROM car_rental_company_discount_plan
                                           WHERE car_type=t.car_type
                                           AND duration_type=(CASE WHEN t.duration >= 90 THEN '90일 이상'
                                                                   WHEN t.duration >= 30 THEN '30일 이상'
                                                                   ELSE '7일 이상' END)),
                 (duration * daily_fee))) AS fee

FROM (SELECT history_id, h.car_id, c.car_type, c.daily_fee, 
             (TIMESTAMPDIFF(DAY, start_date, end_date)+1) AS duration
      FROM car_rental_company_rental_history AS h
      JOIN car_rental_company_car AS c ON c.car_id=h.car_id
      WHERE c.car_type='트럭') AS t

ORDER BY fee DESC, history_id DESC
;
```

## 풀이

조건을 구하는 쿼리를 만든다음 조립하는 방식으로 풀었다.  

이렇게 복잡하게 해야하나? 싶은데 현재로써는 더 나은 쿼리가 생각나지 않는다.  

1. ```history``` 테이블과 ```car``` 테이블을 조인하고 ```car_type```이 ```트럭```인 것만 필터링한다.  
```TIMESTAMPDIFF```를 사용해 대여일수(duration)를 구한다.  
여기서 +1을 하는 이유는 ```start_date```와 ```end_date```가 같으면 0일로 계산되기 때문이다.  
```sql
SELECT history_id, h.car_id, c.car_type, c.daily_fee, 
             (TIMESTAMPDIFF(DAY, start_date, end_date)+1) AS duration
FROM car_rental_company_rental_history AS h
JOIN car_rental_company_car AS c ON c.car_id=h.car_id
WHERE c.car_type='트럭') AS t
```

2. ```plan```테이블에서 조건에 맞는 할인율을 구한다.  
```discount_rate```가 정수이기 때문에 100에서 빼준 뒤 0.001을 곱한다.  
이후 대여금에 곱할 것이다.  
```sql
SELECT (100-discount_rate) * 0.01
FROM car_rental_company_discount_plan
WHERE car_type=t.car_type
AND duration_type=(CASE WHEN t.duration >= 90 THEN '90일 이상'
                        WHEN t.duration >= 30 THEN '30일 이상'
                        ELSE '7일 이상' END)
```

3. 2번에서 구한 할인율을 대여금에 곱해준다.  
```IF```문 사용해서 대여기간이 7일 이상인 경우만 할인을 적용한다.  
```FLOOR```문으로 소수점아래는 잘라낸다.  
```sql
FLOOR(IF (duration >= 7, 
         (duration * daily_fee) * (2번 쿼리로 구한 할인율),
         (duration * daily_fee))) AS fee
```

3. 전체를 조립하고 조건에 맞게 정렬해준다.  
```sql
ORDER BY fee DESC, history_id DESC
```
