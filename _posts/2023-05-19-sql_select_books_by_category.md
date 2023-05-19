---
published: True
title:  "[Programmers, MySQL, Lv.1] 조건에 맞는 도서 리스트 출력하기"
categories: [Programmers, MySQL]
tag: [Programmers, MySQL, Database, SQL]
---

## 문제

> BOOK 테이블에서 2021년에 출판된 '인문' 카테고리에 속하는 도서 리스트를 찾아서 도서 ID(BOOK_ID), 출판일 (PUBLISHED_DATE)을 출력하는 SQL문을 작성해주세요.
결과는 출판일을 기준으로 오름차순 정렬해주세요.


## 쿼리

```sql
SELECT book_id, DATE_FORMAT(published_date, '%Y-%m-%d') AS published_date
FROM book
WHERE category='인문'
AND YEAR(published_date)=2021
ORDER BY published_date ASC
;
```

## 풀이

1. ```category```가 ```경제```이고 ```출판년도```가  ```2021```년인 데이터만 필터링한다.  
```sql
WHERE category='인문'
AND YEAR(published_date)=2021
```

2. ```DATE_FORMAT```을 사용해 날자 포맷을 설정.
```sql
DATE_FORMAT(published_date, '%Y-%m-%d') AS published_date
```

3. ```published_date``` 기준으로 오름차순 정렬
```sql
ORDER BY published_date ASC
```