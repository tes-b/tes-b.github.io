---
published: True
title:  "[Programmers, MySQL, Lv.3] 대여 횟수가 많은 자동차들의 월별 대여 횟수 구하기
도움말"
categories: [Programmers, MySQL]
tag: [Programmers, MySQL, Database, SQL]
---

## 문제

> CAR_RENTAL_COMPANY_RENTAL_HISTORY 테이블에서 대여 시작일을 기준으로 2022년 8월부터 2022년 10월까지 총 대여 횟수가 5회 이상인 자동차들에 대해서 해당 기간 동안의 월별 자동차 ID 별 총 대여 횟수(컬럼명: RECORDS) 리스트를 출력하는 SQL문을 작성해주세요. 결과는 월을 기준으로 오름차순 정렬하고, 월이 같다면 자동차 ID를 기준으로 내림차순 정렬해주세요. 특정 월의 총 대여 횟수가 0인 경우에는 결과에서 제외해주세요.

## 해답

```sql
SELECT MONTH(start_date) AS month, car_id, COUNT(history_id) AS records
FROM car_rental_company_rental_history
WHERE car_id in (SELECT car_id
                    FROM car_rental_company_rental_history
                    WHERE MONTH(start_date) IN (8,9,10)
                    GROUP BY car_id
                    HAVING COUNT(history_id) >= 5)
AND MONTH(start_date) IN (8,9,10)
GROUP BY car_id, month
ORDER BY month ASC, car_id DESC
;
```

## 풀이

1. 8,9,10월의 대여 횟수가 5회 이상인 ```car_id```리스트를 서브 쿼리로 구한다.  
```sql
SELECT car_id
FROM car_rental_company_rental_history
WHERE MONTH(start_date) IN (8,9,10)
GROUP BY car_id
HAVING COUNT(history_id) >= 5
```

2. 구한 ```car_id```목록에 포함되는 항목만 필터링 한다.   
```sql
WHERE car_id in (1번 서브쿼리)
```

3. ```GROUP BY```를 사용해 ```car_id```와 ```MONTH(start_date)``` 기준으로 그룹화 한다.  
```sql
GROUP BY car_id, month
```

4. ```SELECT``` 로 기준 컬럼을 선택하고 이름을 ```month```와 ```records```로  설정한다.
```sql
SELECT MONTH(start_date) AS month, car_id, COUNT(history_id) AS records
```


4. ```month``` 기준으로 오름차순 ```car_id```기준으로 내림차순 정렬한다.  
```sql
ORDER BY month ASC, car_id DESC
```