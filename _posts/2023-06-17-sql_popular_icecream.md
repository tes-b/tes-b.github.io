---
published: True
title:  "[Programmers, MySQL, Lv.1] 인기있는 아이스크림"
categories: [Programmers, MySQL]
tag: [Programmers, MySQL, Database, SQL]
---


## 문제

> 상반기에 판매된 아이스크림의 맛을 총주문량을 기준으로 내림차순 정렬하고 총주문량이 같다면 출하 번호를 기준으로 오름차순 정렬하여 조회하는 SQL 문을 작성해주세요.  

## 쿼리

```sql
SELECT flavor
FROM first_half
ORDER BY total_order DESC, shipment_id ASC
```

## 풀이

정렬만 하면 되는 문제다. 

1. ```ORDER BY```로 조건에 맞게 정렬해준다.    
```sql
ORDER BY total_order DESC, shipment_id ASC
```