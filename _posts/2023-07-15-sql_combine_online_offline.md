---
published: True
title:  "[Programmers, MySQL, Lv.4] 오프라인/온라인 판매 데이터 통합하기"
categories: [Programmers, MySQL]
tag: [Programmers, MySQL, Database, SQL]
---


## 문제

> ONLINE_SALE 테이블과 OFFLINE_SALE 테이블에서 2022년 3월의 오프라인/온라인 상품 판매 데이터의 판매 날짜, 상품ID, 유저ID, 판매량을 출력하는 SQL문을 작성해주세요. OFFLINE_SALE 테이블의 판매 데이터의 USER_ID 값은 NULL 로 표시해주세요. 결과는 판매일을 기준으로 오름차순 정렬해주시고 판매일이 같다면 상품 ID를 기준으로 오름차순, 상품ID까지 같다면 유저 ID를 기준으로 오름차순 정렬해주세요.

## 쿼리

첫번째 방법
```sql
SELECT DATE_FORMAT(sales_date, '%Y-%m-%d') AS sales_date, product_id, user_id, sales_amount
FROM (  SELECT sales_date, product_id, user_id, sales_amount
        FROM online_sale
        UNION ALL
        SELECT sales_date, product_id, NULL , sales_amount
        FROM offline_sale) AS t
WHERE DATE_FORMAT(sales_date,'%Y-%m') = '2022-03'
ORDER BY sales_date ASC, product_id ASC, user_id ASC
```

두번째 방법
```sql
SELECT DATE_FORMAT(sales_date, '%Y-%m-%d') AS sales_date, product_id, user_id, sales_amount
FROM online_sale
WHERE DATE_FORMAT(sales_date,'%Y-%m') = '2022-03'

UNION ALL

SELECT DATE_FORMAT(sales_date, '%Y-%m-%d') AS sales_date, product_id, NULL AS user_id, sales_amount
FROM offline_sale
WHERE DATE_FORMAT(sales_date,'%Y-%m') = '2022-03'

ORDER BY sales_date ASC, product_id ASC, user_id ASC
```

## 풀이

첫번째 방법 기준으로 작성

1. ```online_sale``` 테이블과  ```offline_sale``` 테이블을 ```UNION ALL```로 합친다.  
```offline_sale``` 테이블은 ```user_id```를 ```NULL``` 로 표시하는 것이 문제 조건이다.  
```sql
FROM (  SELECT sales_date, product_id, user_id, sales_amount
        FROM online_sale
        UNION ALL
        SELECT sales_date, product_id, NULL , sales_amount
        FROM offline_sale) AS t
```

2. 합친 테이블에서 ```sales_date```가 2022년 03월인 행만 필터링한다.     
```sql
WHERE DATE_FORMAT(sales_date,'%Y-%m') = '2022-03'
```

3. ```DATE_FORMAT```으로 날짜를 양식에 맞춘다.  
```sql
SELECT DATE_FORMAT(sales_date, '%Y-%m-%d') AS sales_date, product_id, user_id, sales_amount
```

4. ```sales_date```, ```product_id```, ```user_id```를 오름차순으로 정렬한다.  
```sql
ORDER BY sales_date ASC, product_id ASC, user_id ASC
```