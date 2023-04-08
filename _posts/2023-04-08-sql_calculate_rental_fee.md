---
published: True
title:  "[Programmers, MySQL, Lv.4] 특정 기간동안 대여 가능한 자동차들의 대여비용 구하기"
categories: CodingTests
tag: [Programmers, MySQL, Database, SQL]
---

## 문제

> CAR_RENTAL_COMPANY_CAR 테이블과 CAR_RENTAL_COMPANY_RENTAL_HISTORY 테이블과 CAR_RENTAL_COMPANY_DISCOUNT_PLAN 테이블에서 자동차 종류가 '세단' 또는 'SUV' 인 자동차 중 2022년 11월 1일부터 2022년 11월 30일까지 대여 가능하고 30일간의 대여 금액이 50만원 이상 200만원 미만인 자동차에 대해서 자동차 ID, 자동차 종류, 대여 금액(컬럼명: FEE) 리스트를 출력하는 SQL문을 작성해주세요. 결과는 대여 금액을 기준으로 내림차순 정렬하고, 대여 금액이 같은 경우 자동차 종류를 기준으로 오름차순 정렬, 자동차 종류까지 같은 경우 자동차 ID를 기준으로 내림차순 정렬해주세요.

## 해답

``` SQL
SELECT car_id, car_type, fee
FROM (SELECT c.car_id, c.car_type,
    FLOOR(daily_fee * 30 * (SELECT (100-discount_rate) * 0.01
                        FROM car_rental_company_discount_plan
                        where car_type=c.car_type AND duration_type='30일 이상')
    ) AS fee
    FROM car_rental_company_car AS c
    WHERE car_type IN ('세단', 'SUV')
    AND car_id NOT IN (SELECT car_id
                        FROM car_rental_company_rental_history
                        WHERE (start_date <= '2022-11-30 00:00:00')
                        AND (end_date >= '2022-11-01 00:00:00')
                        GROUP BY car_id)
    ) AS t
WHERE fee >= 500000 AND fee < 2000000
ORDER BY fee DESC, car_type ASC, car_id DESC
;
```

## 풀이

조건을 구하는 쿼리를 만든다음 조립하는 방식으로 풀었다.  

1. 대여 불가능한 차량 id를 구하는 서브쿼리  
```SQL
SELECT car_id
FROM car_rental_company_rental_history
WHERE (start_date <= '2022-11-30 00:00:00')
AND (end_date >= '2022-11-01 00:00:00')
GROUP BY car_id
;
```
```car_rental_company_rental_history```에서  
```start_date```가 11월 30이전이면서  
```end_date```가 11월 01일 이후인 기록들의 ```car_id```만 가져온다.  

2. 1번의 서브쿼리에서 나온 ```car_id```가 아닌 것들 중  
```세단```과 ```SUV```만 가젼온다.  
```daily_fee```에 30일을 곱해 ```fee```를 구한다. 이후 할인율을 곱해줄 것이다.  
```sql
SELECT c.car_id, c.car_type, FLOOR(daily_fee * 30) AS fee
    FROM car_rental_company_car AS c
    WHERE car_type IN ('세단', 'SUV')
    AND car_id NOT IN (SELECT car_id
                        FROM car_rental_company_rental_history
                        WHERE (start_date <= '2022-11-30 00:00:00')
                        AND (end_date >= '2022-11-01 00:00:00')
                        GROUP BY car_id)
;
```

3. 할인율을 구하는 서브쿼리  
```sql
SELECT (100-discount_rate) * 0.01
FROM car_rental_company_discount_plan
where car_type=c.car_type AND duration_type='30일 이상'
;
```
```car_rental_company_discount_plan```에서 할인율을 가져온다.  
```duration_type```은 ```30일 이상```인것을 가져오고  
```car_type```은 해당하는 것을 가져올 것이다.  
할인율은 %단위의 정수로 되어있기 때문에  
100에서 할인율을 빼고 0.01을 곱해서 바로 전체 금액에 곱할 것이다.  
할인율이 8% 라면 ```(100-8)*0.01 = 0.92```가 된다.  

4. 위의 서브쿼리로 나온 할인율을 곱해준다.  
```sql
SELECT c.car_id, c.car_type,
    FLOOR(daily_fee * 30 * (SELECT (100-discount_rate) * 0.01
                        FROM car_rental_company_discount_plan
                        where car_type=c.car_type AND duration_type='30일 이상')
    ) AS fee
    FROM car_rental_company_car AS c
    WHERE car_type IN ('세단', 'SUV')
    AND car_id NOT IN (SELECT car_id
                        FROM car_rental_company_rental_history
                        WHERE (start_date <= '2022-11-30 00:00:00')
                        AND (end_date >= '2022-11-01 00:00:00')
                        GROUP BY car_id)
;
```

5. 전체 테이블에서 비용 50만원 이상 200만원 미만인 것만 필터링하고 조건에 맞게 정렬한다. 
```sql
SELECT car_id, car_type, fee
FROM (`4번까지 진행한 테이블`) AS t
WHERE fee >= 500000 AND fee < 2000000
ORDER BY fee DESC, car_type ASC, car_id DESC
```