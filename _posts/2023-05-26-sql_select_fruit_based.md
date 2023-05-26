---
published: True
title:  "[Programmers, MySQL, Lv.1] 과일로 만든 아이스크림 고르기"
categories: [Programmers, MySQL]
tag: [Programmers, MySQL, Database, SQL]
---

## 문제

> 상반기 아이스크림 총주문량이 3,000보다 높으면서 아이스크림의 주 성분이 과일인 아이스크림의 맛을 총주문량이 큰 순서대로 조회하는 SQL 문을 작성해주세요.

## 쿼리

```sql
SELECT first_half.flavor
FROM first_half 
JOIN icecream_info
ON first_half.flavor = icecream_info.flavor
WHERE ingredient_type = 'fruit_based'
AND total_order >= 3000
ORDER BY total_order DESC
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

2. ```ingredient_type```이 ```fruit_based```고  
```total_order```가 ```3000``` 이상인 경우만 필터링한다..   
```sql
WHERE ingredient_type = 'fruit_based'
```

3. ```total_order``` 내림차순으로 정렬한다.  
```sql
ORDER BY total_order DESC
```