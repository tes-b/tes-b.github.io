---
published: True
title:  "[Programmers, MySQL, Lv.1] 조건에 부합하는 중고거래 댓글 조회하기"
categories: CodingTests
tag: [Programmers, MySQL, Database, SQL]
---

## 문제

> USED_GOODS_BOARD와 USED_GOODS_REPLY 테이블에서 2022년 10월에 작성된 게시글 제목, 게시글 ID, 댓글 ID, 댓글 작성자 ID, 댓글 내용, 댓글 작성일을 조회하는 SQL문을 작성해주세요. 결과는 댓글 작성일을 기준으로 오름차순 정렬해주시고, 댓글 작성일이 같다면 게시글 제목을 기준으로 오름차순 정렬해주세요.

## 해답

```sql
SELECT B.TITLE, B.BOARD_ID, R.REPLY_ID, R.WRITER_ID, R.CONTENTS, DATE_FORMAT(R.CREATED_DATE, "%Y-%m-%d") AS CREATED_DATE
FROM USED_GOODS_REPLY AS R
JOIN USED_GOODS_BOARD AS B ON R.BOARD_ID = B.BOARD_ID
WHERE DATE_FORMAT(B.CREATED_DATE, "%Y-%m") = "2022-10"
ORDER BY R.CREATED_DATE ASC, B.TITLE ASC;
```

## 풀이

```JOIN```를 사용해 ```BOARD_ID```를 키 값으로 두 테이블을 합친다.  

조건절에서 예시처럼 ```DATE_FORMAT```을 사용해도 되고 아래처럼 해도 무관하다.  
```sql
WHERE YEAR(B.CREATED_DATE) = 2022 AND MONTH(B.CREATED_DATE) = 10
```  
```CREATED_DATE``` 항목을 ```yyyy-mm-dd``` 형식으로 맞춰주기 위해 ```DATE_FORMAT```을 사용한다.  
