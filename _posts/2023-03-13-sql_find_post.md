---
published: True
title:  "[Programmers, MySQL, Lv.2] 조건에 부합하는 중고거래 상태 조회하기"
categories: [Programmers, MySQL]
tag: [Programmers, MySQL, Database, SQL]
---

## 문제

> USED_GOODS_BOARD 테이블에서 2022년 10월 5일에 등록된 중고거래 게시물의 게시글 ID, 작성자 ID, 게시글 제목, 가격, 거래상태를 조회하는 SQL문을 작성해주세요. 거래상태가 SALE 이면 판매중, RESERVED이면 예약중, DONE이면 거래완료 분류하여 출력해주시고, 결과는 게시글 ID를 기준으로 내림차순 정렬해주세요.

## 해답

```sql
SELECT BOARD_ID, WRITER_ID, TITLE, PRICE, 
CASE WHEN (STATUS="SALE")       THEN "판매중"
     WHEN (STATUS="RESERVED")   THEN "예약중"
     ELSE "거래완료" 
     END AS STATUS
FROM USED_GOODS_BOARD
WHERE DATE_FORMAT(CREATED_DATE, "%Y-%m-%d") = "2022-10-05"
ORDER BY BOARD_ID DESC;
```

## 풀이

```CASE```문을 이용해 ```STATUS```를 분류해서 출력한다.  
```DATE_FORMAT```으로 날짜를 필터링한다.  
```ORDER BY```로 ```BOARD_ID```기준으로 내림차순 ```DESC``` 정렬한다.  
