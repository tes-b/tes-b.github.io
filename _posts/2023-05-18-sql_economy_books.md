---
published: True
title:  "[Programmers, MySQL, Lv.2] 조건에 맞는 도서와 저자 리스트 출력하기"
categories: [Programmers, MySQL]
tag: [Programmers, MySQL, Database, SQL]
---

## 문제

> '경제' 카테고리에 속하는 도서들의 도서 ID(BOOK_ID), 저자명(AUTHOR_NAME), 출판일(PUBLISHED_DATE) 리스트를 출력하는 SQL문을 작성해주세요.
결과는 출판일을 기준으로 오름차순 정렬해주세요.

## 쿼리

```sql
SELECT book_id, author_name, DATE_FORMAT(published_date, '%Y-%m-%d') AS published_date
FROM book
JOIN author ON book.author_id=author.author_id
WHERE category='경제'
ORDER BY published_date ASC
;
```

## 풀이

1. ```JOIN```으로 ```book```과 ```author``` 테이블을 합친다.  
```sql
FROM book
JOIN author ON book.author_id=author.author_id
```

2. ```category```가 ```경제```인 데이터만 필터링한다.  
```sql
WHERE category='경제'
```

3. ```DATE_FORMAT```을 사용해 날자 포맷을 설정.
```sql
DATE_FORMAT(published_date, '%Y-%m-%d') AS published_date
```

4. ```published_date``` 기준으로 오름차순 정렬
```sql
ORDER BY published_date ASC
```