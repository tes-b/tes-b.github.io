---
published: True
title:  "[Programmers, MySQL, Lv.3] 카테고리 별 도서 판매량 집계하기"
categories: [Programmers, MySQL]
tag: [Programmers, MySQL, Database, SQL]
---

## 문제

> 2022년 1월의 카테고리 별 도서 판매량을 합산하고, 카테고리(CATEGORY), 총 판매량(TOTAL_SALES) 리스트를 출력하는 SQL문을 작성해주세요.
결과는 카테고리명을 기준으로 오름차순 정렬해주세요.

## 해답

```sql
SELECT category, SUM(sales) AS total_sales 
FROM book 
JOIN book_sales ON book.book_id = book_sales.book_id
WHERE DATE_FORMAT(sales_date,'%Y-%m') = '2022-01'
GROUP BY category
ORDER BY category
;
```

## 풀이

1. ```JOIN```으로 ```book```과 ```book_sales``` 테이블을 합친다.  
```sql
FROM book 
JOIN book_sales ON book.book_id = book_sales.book_id
```

2. 2022년 01월의 판매 데이터만 필터링한다.  
```sql
WHERE DATE_FORMAT(sales_date,'%Y-%m') = '2022-01'
```

3. ```catergory```로 ```GROUP BY```
```sql
GROUP BY category
```
4. ```sales``` 항목 합계
```sql
SELECT category, SUM(sales) AS total_sales 
```

5. ```category``` 기준으로 오름차순 정렬
```sql
ORDER BY category
```