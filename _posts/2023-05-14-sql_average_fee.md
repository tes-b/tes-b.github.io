---
published: True
title:  "[Programmers, MySQL, Lv.1] 평균 일일 대여 요금 구하기"
categories: [Programmers, MySQL]
tag: [Programmers, MySQL, Database, SQL]
---

## 문제

> CAR_RENTAL_COMPANY_CAR 테이블에서 자동차 종류가 'SUV'인 자동차들의 평균 일일 대여 요금을 출력하는 SQL문을 작성해주세요. 이때 평균 일일 대여 요금은 소수 첫 번째 자리에서 반올림하고, 컬럼명은 AVERAGE_FEE 로 지정해주세요.

## 해답

```sql
SELECT ROUND(AVG(daily_fee),0) AS average_fee
FROM car_rental_company_car
WHERE car_type = 'SUV'
;
```

## 풀이

1. ```car_type```이 ```SUV``` 인 경우만 필터링
```sql
WHERE car_type = 'SUV'
```

2. ```AVG```를 사용해 ```daily_fee```의 평균을 구한다. 

3. ```ROUND``` 로 첫째자리에서 반올림한다.  
소수 첫째자리에서 반올림하면 소수 부분은 없어지고 정수부분만 남아야 한다.  
```sql
ROUND(AVG(daily_fee),0)
```