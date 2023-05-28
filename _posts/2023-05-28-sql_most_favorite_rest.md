---
published: True
title:  "[Programmers, MySQL, Lv.3] 즐겨찾기가 가장 많은 식당 정보 출력하기"
categories: [Programmers, MySQL]
tag: [Programmers, MySQL, Database, SQL]
---

## 문제

> REST_INFO 테이블에서 음식종류별로 즐겨찾기수가 가장 많은 식당의 음식 종류, ID, 식당 이름, 즐겨찾기수를 조회하는 SQL문을 작성해주세요. 이때 결과는 음식 종류를 기준으로 내림차순 정렬해주세요.

## 쿼리

```sql
SELECT food_type, rest_id, rest_name, favorites
FROM rest_info
JOIN (SELECT MAX(favorites) AS max_favorites
      FROM rest_info
      GROUP BY food_type) AS t
ON rest_info.favorites = t.max_favorites
GROUP BY food_type
ORDER BY food_type DESC
;
```

## 풀이

1. ```food_type```별로 가장 많은```favorites``` 수를 뽑는다.  
```sql
(SELECT MAX(favorites) AS max_favorites
FROM rest_info
GROUP BY food_type) AS t
```

```
max_favorites
734
230
102
151
20
```

2. ```1```번에서 뽑은 테이블과 ```rest_info``` 테이블을  
```favorites``` 기준으로 조인한다.
```sql
FROM rest_info
JOIN (SELECT MAX(favorites) AS max_favorites
      FROM rest_info
      GROUP BY food_type) AS t
ON rest_info.favorites = t.max_favorites
```

3. ```food_type``` 기준으로 ```GROUP BY```  
```sql
GROUP BY food_type
```

3. ```food_type``` 기준으로 내림차순 정렬  
```sql
ORDER BY food_type DESC
```