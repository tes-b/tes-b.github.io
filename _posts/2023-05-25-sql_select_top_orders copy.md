---
published: True
title:  "[Programmers, MySQL, Lv.2] 성분으로 구분한 아이스크림 총 주문량"
categories: [Programmers, MySQL]
tag: [Programmers, MySQL, Database, SQL]
---

## 문제

> 상반기 동안 각 아이스크림 성분 타입과 성분 타입에 대한 아이스크림의 총주문량을 총주문량이 작은 순서대로 조회하는 SQL 문을 작성해주세요. 이때 총주문량을 나타내는 컬럼명은 TOTAL_ORDER로 지정해주세요.

## 쿼리

```sql
SELECT ingredient_type , SUM(total_order) AS total_order
FROM first_half
JOIN icecream_info ON first_half.flavor = icecream_info.flavor
GROUP BY ingredient_type
ORDER BY total_order ASC
;
```

## 풀이

1. ```first_half```와 ```icecream_info``` 테이블을  
```flavor``` 기준으로 ```JOIN```한다.  
```sql
FROM first_half
JOIN icecream_info 
ON first_half.flavor = icecream_info.flavor
```

2. ```ingredient_type```을 ```GROUP BY```로 묶는다.  
```sql
GROUP BY ingredient_type
```

3. ```total_order```항목을 ````SUM```으로 합계를 구한다.
```sql
SELECT ingredient_type , SUM(total_order) AS total_order
```

4. ```total_order``` 오름차순으로 정렬한다.  
```sql
ORDER BY total_order ASC
```