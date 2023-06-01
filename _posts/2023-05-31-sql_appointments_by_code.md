---
published: True
title:  "[Programmers, MySQL, Lv.2] 진료과별 총 예약 횟수 출력하기"
categories: [Programmers, MySQL]
tag: [Programmers, MySQL, Database, SQL]
---

## 문제

> APPOINTMENT 테이블에서 2022년 5월에 예약한 환자 수를 진료과코드 별로 조회하는 SQL문을 작성해주세요. 이때, 컬럼명은 '진료과 코드', '5월예약건수'로 지정해주시고 결과는 진료과별 예약한 환자 수를 기준으로 오름차순 정렬하고, 예약한 환자 수가 같다면 진료과 코드를 기준으로 오름차순 정렬해주세요.

## 쿼리

```sql
SELECT mcdp_cd AS '진료과코드', COUNT(*) AS '5월예약건수'
FROM appointment
WHERE DATE_FORMAT(apnt_ymd,'%y-%m') = '22-05'
GROUP BY mcdp_cd
ORDER BY COUNT(*) ASC, mcdp_cd ASC
;
```

## 풀이

1. 예약 날짜가 ```2022년 5월```인 경우를 필터링한다.    
```sql
WHERE DATE_FORMAT(apnt_ymd,'%y-%m') = '22-05'
```
2. ```mcdp_cd```(진료과 코드) 기준으로 ```GROUP BY```  
```sql
GROUP BY mcdp_cd
```

3. ```COUNT(*)```와 ```mcdp_cd``` 기준으로 오름차순 정렬.
```sql
ORDER BY COUNT(*) ASC, mcdp_cd ASC
```

4. ```mcdp_cd```를 ```진료과코드```로 ```COUNT(*)```를 ```5월예약건수```로 지정
```sql
SELECT mcdp_cd AS '진료과코드', COUNT(*) AS '5월예약건수'
```