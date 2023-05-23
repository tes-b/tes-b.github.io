---
published: True
title:  "[Programmers, MySQL, Lv.4] 주문량이 많은 아이스크림들 조회하기"
categories: [Programmers, MySQL]
tag: [Programmers, MySQL, Database, SQL]
---

## 문제

> 7월 아이스크림 총 주문량과 상반기의 아이스크림 총 주문량을 더한 값이 큰 순서대로 상위 3개의 맛을 조회하는 SQL 문을 작성해주세요.

## 쿼리

```sql
SELECT FLAVOR
FROM (SELECT * FROM first_half
      UNION ALL
      SELECT * FROM july) AS combined
GROUP BY FLAVOR
ORDER BY SUM(TOTAL_ORDER) DESC
LIMIT 3
;
```

## 풀이

1. 데이블 두개의 스키마가 같고 중복되는 내용이 없기 때문에  
```UNION ALL```로 두 테이블을 세로로 붙인다. 
```sql
SELECT * FROM first_half
UNION ALL
SELECT * FROM july
```

2. ```GROUP BY```로 ```FLAVOR``` 별로 그룹화
```sql
GROUP BY FLAVOR
```

3. ```SUM(TOTAL_ORDER)``` 전체 주문량 기준으로 오름차순 정렬
```sql
ORDER BY SUM(TOTAL_ORDER) DESC
```

4. 주문량 높은 3개만 표시
```sql
LIMIT 3
```