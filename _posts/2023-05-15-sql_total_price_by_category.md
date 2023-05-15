---
published: True
title:  "[Programmers, MySQL, Lv.4] 저자 별 카테고리 별 매출액 집계하기"
categories: [Programmers, MySQL]
tag: [Programmers, MySQL, Database, SQL]
---

## 문제

> 2022년 1월의 도서 판매 데이터를 기준으로 저자 별, 카테고리 별 매출액(TOTAL_SALES = 판매량 * 판매가) 을 구하여, 저자 ID(AUTHOR_ID), 저자명(AUTHOR_NAME), 카테고리(CATEGORY), 매출액(SALES) 리스트를 출력하는 SQL문을 작성해주세요.
결과는 저자 ID를 오름차순으로, 저자 ID가 같다면 카테고리를 내림차순 정렬해주세요.

## 해답

```sql
SELECT book.author_id, author.author_name, category, SUM(sales * price) AS total_price
FROM book_sales
JOIN book ON book.book_id = book_sales.book_id
JOIN author ON book.author_id = author.author_id
WHERE DATE_FORMAT(sales_date,'%Y-%m') = '2022-01'
GROUP BY author_id, category
ORDER BY author.author_id ASC, category DESC
;
```

## 풀이

1. ```JOIN```으로 모든 테이블을 합친다.  
```sql
FROM book_sales
JOIN book ON book.book_id = book_sales.book_id
JOIN author ON book.author_id = author.author_id
```

2. 2022년 01월의 판매 데이터만 필터링한다.  
```sql
WHERE DATE_FORMAT(sales_date,'%Y-%m') = '2022-01'
```

3. ```author_id```와 ```catergory```로 ```GROUP BY```
```sql
GROUP BY author_id, category
```
4. ```sales```와 ```price```를 곱해서 ```total_price``` 계산
```sql
SUM(sales * price) AS total_price
```

5. ```author_id``` 오름차순 ```category``` 내ㄹ미차순으로 정렬
```sql
ORDER BY author.author_id ASC, category DESC
```