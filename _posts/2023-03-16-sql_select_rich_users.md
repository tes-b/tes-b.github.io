---
published: True
title:  "[Programmers, MySQL, Lv.3] 조건에 맞는 사용자 정보 조회하기"
categories: CodingTests
tag: [Programmers, MySQL, Database, SQL]
---

이 문제는 제목이 앞에서 풀었던 문제와 서로 바뀐 것 같다.  
프로그래머스에 문의를 넣었다.  
아무튼 문제의 요구조건을 충족하면 된다.  

## 문제

> USED_GOODS_BOARD와 USED_GOODS_USER 테이블에서 완료된 중고 거래의 총금액이 70만 원 이상인 사람의 회원 ID, 닉네임, 총거래금액을 조회하는 SQL문을 작성해주세요. 결과는 총거래금액을 기준으로 오름차순 정렬해주세요.

## 해답

``` SQL
SELECT USER_ID, NICKNAME, SUM(PRICE) AS TOTAL_SALES
FROM USED_GOODS_BOARD AS B
JOIN USED_GOODS_USER AS U 
ON B.WRITER_ID = U.USER_ID
WHERE STATUS = 'DONE'
GROUP BY USER_ID
HAVING TOTAL_SALES >= 700000
ORDER BY TOTAL_SALES ASC;
```

## 풀이

1. ```JOIN```으로 ```WRITER_ID```와 ```USER_ID```를 기준으로 두 테이블을 합친다.  
2. ```WHERE```에서 ```STATUS```항목이 ```DONE```(거래완료) 항목을 추려낸다.   
3. ```USER_ID``` 기준으로 ```GROUP BY```로 묶고 ```SUM```이용해 합계를 구한다.  
4. ```HAVING``` 조건을 사용해 총 거래금액이 70만원 이상인 것 만 추려낸다.  
5. ```ORDER BY```로 총 거래금액 기준으로 오름차순 ```ASC``` 정렬한다.  
