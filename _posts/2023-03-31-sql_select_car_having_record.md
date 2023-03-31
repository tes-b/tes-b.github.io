---
published: True
title:  "[Programmers, MySQL, Lv.3] 대여 기록이 존재하는 자동차 리스트 구하기"
categories: CodingTests
tag: [Programmers, MySQL, Database, SQL]
---

## 문제

> CAR_RENTAL_COMPANY_CAR 테이블과 CAR_RENTAL_COMPANY_RENTAL_HISTORY 테이블에서 자동차 종류가 '세단'인 자동차들 중 10월에 대여를 시작한 기록이 있는 자동차 ID 리스트를 출력하는 SQL문을 작성해주세요. 자동차 ID 리스트는 중복이 없어야 하며, 자동차 ID를 기준으로 내림차순 정렬해주세요.

## 해답

``` SQL
SELECT DISTINCT H.CAR_ID 
FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY AS H
JOIN CAR_RENTAL_COMPANY_CAR AS C
ON C.CAR_ID = H.CAR_ID
WHERE MONTH(START_DATE)=10
AND CAR_TYPE = '세단'
ORDER BY H.CAR_ID DESC
;
```

## 풀이

1. ```JOIN```을 사용해 ```CAR_RENTAL_COMPANY_RENTAL_HISTORY``` 테이블과  
```CAR_RENTAL_COMPANY_RENTAL_HISTORY``` 테이블을 ```CAR_ID```를 기준으로 합친다.  
2. ```MONTH```를 사용해 10월 대여기록이 있는 기록 필터링.  
3. ```CAR_TYPE```이 ```세단```인 기록 필터링.  
4. ```ORDER BY```로 ```CAR_ID``` 내림차순 정렬.  
