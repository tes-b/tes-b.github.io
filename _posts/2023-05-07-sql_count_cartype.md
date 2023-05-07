---
published: True
title:  "[Programmers, MySQL, Lv.2] 자동차 종류 별 특정 옵션이 포함된 자동차 수 구하기"
categories: [Programmers, MySQL]
tag: [Programmers, MySQL, Database, SQL]
---

## 문제

> CAR_RENTAL_COMPANY_CAR 테이블에서 '통풍시트', '열선시트', '가죽시트' 중 하나 이상의 옵션이 포함된 자동차가 자동차 종류 별로 몇 대인지 출력하는 SQL문을 작성해주세요. 이때 자동차 수에 대한 컬럼명은 CARS로 지정하고, 결과는 자동차 종류를 기준으로 오름차순 정렬해주세요.

## 해답

```sql
SELECT car_type, COUNT(car_id) AS cars
FROM car_rental_company_car
WHERE options LIKE '%시트%'
GROUP BY car_type
ORDER BY car_type ASC
;
```

## 풀이

1. '통풍시트', '열선시트', '가죽시트' 세가지 시트가 포함되어야 한다.  
```options```에 위의 3개 말고 다른 시트가 있다면 ```WHERE```문을 아래와 같이 작성해야 하지만  
```sql
WHERE options LIKE '%통풍시트%'
OR options LIKE '%열선시트%'
OR options LIKE '%가죽시트%'
```  
3가지 종류가 모든 시트의 전부이기 때문에  
```sql
WHERE options LIKE '%시트%'
```   
요렇게만 해주면 된다.  

2. ```GROUP BY```를 사용해 ```car_type```으로 그룹화한다.  

3. ```COUNT``` 로 ```car_type```별 합계를 구한다.  

4. ```car_type``` 기준으로 오름차순 정렬한다.  
```sql
ORDER BY car_type ASC
```