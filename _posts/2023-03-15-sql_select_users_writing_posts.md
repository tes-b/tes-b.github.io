---
published: True
title:  "[Programmers, MySQL, Lv.3] 조건에 맞는 사용자와 총 거래금액 조회하기"
categories: CodingTests
tag: [Programmers, MySQL, Database, SQL]
---

이 문제는 제목이 좀 이상한데 총 거래금액 조회라고 하면서 거래금액은 전혀 조회하지 않는다.  
아무튼 문제의 요구조건을 충족하면 된다.  

## 문제

> USED_GOODS_BOARD와 USED_GOODS_USER 테이블에서 중고 거래 게시물을 3건 이상 등록한 사용자의 사용자 ID, 닉네임, 전체주소, 전화번호를 조회하는 SQL문을 작성해주세요. 이때, 전체 주소는 시, 도로명 주소, 상세 주소가 함께 출력되도록 해주시고, 전화번호의 경우 xxx-xxxx-xxxx 같은 형태로 하이픈 문자열(-)을 삽입하여 출력해주세요. 결과는 회원 ID를 기준으로 내림차순 정렬해주세요.  

## 해답

``` SQL

SELECT DISTINCT
U.USER_ID, 
U.NICKNAME, 
CONCAT_WS(" ", U.CITY,U.STREET_ADDRESS1,U.STREET_ADDRESS2) AS 전체주소, 
CONCAT_WS("-", SUBSTRING(TLNO, 1, 3), SUBSTRING(TLNO, 4, 4), SUBSTRING(TLNO, 8, 4)) AS 전화번호
FROM USED_GOODS_BOARD AS B
JOIN USED_GOODS_USER AS U 
ON B.WRITER_ID = U.USER_ID
WHERE U.USER_ID IN (SELECT WRITER_ID 
        FROM USED_GOODS_BOARD
        GROUP BY WRITER_ID
        HAVING COUNT(*) >= 3)
ORDER BY U.USER_ID DESC;
```

## 풀이

1. ```JOIN```으로 ```WRITER_ID```와 ```USER_ID```를 기준으로 두 테이블을 합친다.  
2. ```WHERE``` 조건절에서 서브쿼리를 만들어야하는데  
```GROUP BY```와 ```HAVING``` 조건을 사용해 ```USED_GOODS_BOARD``` 테이블에서 글이 3개 이상인 아이디를 추려낸다.  
```sql
--subquery
SELECT WRITER_ID 
FROM USED_GOODS_BOARD
GROUP BY WRITER_ID
HAVING COUNT(*) >= 3
```
그다음 서브쿼리 결과에 포함된```USER_ID```만 선택하면 되는데  
이때 중복선택 되는 것들은 ```DISTINCT```를 사용해 하나만 남기고 제거한다.  

3. ```CONCAT_WS```로 주소항목들을 공백으로 이어붙인다. 
```sql
CONCAT_WS(" ", U.CITY,U.STREET_ADDRESS1,U.STREET_ADDRESS2) AS 전체주소, 
```
4. 전화번호 역시 ```CONCAT_WS```를 사용해 하이픈(-)을 넣고 ```SUBSTRING```으로 잘라서 붙인다.  
```sql
CONCAT_WS("-", SUBSTRING(TLNO, 1, 3), SUBSTRING(TLNO, 4, 4), SUBSTRING(TLNO, 8, 4)) AS 전화번호
```
5. ```ORDER BY```로 ```USER_ID```기준으로 내림차순 ```DESC``` 정렬한다.  
