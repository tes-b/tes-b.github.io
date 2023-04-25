---
published: True
title:  "[Programmers, MySQL, Lv.3] 자동차 대여 기록에서 대여중 / 대여 가능 여부 구분하기"
categories: [Programmers, MySQL]
tag: [Programmers, MySQL, Database, SQL]
---

## 문제

> CAR_RENTAL_COMPANY_RENTAL_HISTORY 테이블에서 2022년 10월 16일에 대여 중인 자동차인 경우 '대여중' 이라고 표시하고, 대여 중이지 않은 자동차인 경우 '대여 가능'을 표시하는 컬럼(컬럼명: AVAILABILITY)을 추가하여 자동차 ID와 AVAILABILITY 리스트를 출력하는 SQL문을 작성해주세요. 이때 반납 날짜가 2022년 10월 16일인 경우에도 '대여중'으로 표시해주시고 결과는 자동차 ID를 기준으로 내림차순 정렬해주세요.

## 해답

```sql
SELECT car_id, 
    MAX(CASE WHEN '2022-10-16 00:00:00' BETWEEN DATE(start_date) and DATE(end_date) 
        THEN '대여중'
        ELSE '대여 가능'
        END) AS availability
FROM car_rental_company_rental_history
GROUP BY car_id
ORDER BY car_id DESC
;
```

## 풀이

1. ```BETWEEN```을 사용해  ```START_DATE```와 ```END_DATE```사이에  
기준날짜 ```'2022-10-16 00:00:00'```가 있는지 체크한다. 
2. ```CASE```문으로 기준날짜가 포함되면 ```대여중``` 아니면 ```대여 가능```으로 표시한다.
```sql
CASE WHEN 조건 THEN 부합한 경우 ELSE 아닌 경우 END
```
3. ```GROUP BY```를 사용해 ```CAR_ID```로 묶어준다.  
이 때 ```MAX```를 사용하면 ```대여중``` 항목이 있는경우 ```대여중```으로 없으면 ```대여 가능```으로 표시된다.
4. ```ORDER BY```로 ```CAR_ID``` 내림차순 정렬.  
