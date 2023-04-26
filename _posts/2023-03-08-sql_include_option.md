---
published: True
title:  "[Programmers, MySQL, Lv.1] 특정 옵션이 포함된 자동차 리스트 구하기"
categories: [Programmers, MySQL]
tag: [Programmers, MySQL, Database, SQL]
---

## 문제

> CAR_RENTAL_COMPANY_CAR 테이블에서 '네비게이션' 옵션이 포함된 자동차 리스트를 출력하는 SQL문을 작성해주세요. 결과는 자동차 ID를 기준으로 내림차순 정렬해주세요.

## 해답

```sql
SELECT * 
FROM CAR_RENTAL_COMPANY_CAR
WHERE OPTIONS LIKE '%네비게이션%'
ORDER BY CAR_ID DESC;
```

## 풀이
```LIKE``` 를 사용해 ```OPTION``` 항목에서 '네비게이션이' 포함된 항목을 가져온다.  
```ORDER BY```로 ```CAR_ID``` 내림차순 으로 정렬한다.  