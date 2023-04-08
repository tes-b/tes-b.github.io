---
published: True
title:  "[Programmers, MySQL, Lv.2] 자동차 평균 대여 기간 구하기"
categories: CodingTests
tag: [Programmers, MySQL, Database, SQL]
---

## 문제

> CAR_RENTAL_COMPANY_RENTAL_HISTORY 테이블에서 평균 대여 기간이 7일 이상인 자동차들의 자동차 ID와 평균 대여 기간(컬럼명: AVERAGE_DURATION) 리스트를 출력하는 SQL문을 작성해주세요. 평균 대여 기간은 소수점 두번째 자리에서 반올림하고, 결과는 평균 대여 기간을 기준으로 내림차순 정렬해주시고, 평균 대여 기간이 같으면 자동차 ID를 기준으로 내림차순 정렬해주세요.

## 해답

```sql
SELECT CAR_ID, ROUND(AVG(TIMESTAMPDIFF(DAY, START_DATE, END_DATE)+1),1) AS AVERAGE_DURATION 
FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
GROUP BY CAR_ID
HAVING AVERAGE_DURATION >= 7
ORDER BY AVERAGE_DURATION DESC, CAR_ID DESC
;
```

## 풀이

[Programmers, MySQL, Lv.1] 자동차 대여 기록에서 장기/단기 대여 구분하기. 왜 30이 아닌 29?  
<https://tes-b.github.io/codingtests/backjoon_sql_rent_type/>  

저번에 썼던 이 게시물과 같은 문제가 있었다.  
대여기간을 구하기 위해 ```TIMESTAMPDIFF```를 사용할 경우  
같은날 빌리고 반납한 경우 0일로 계산이 된다.  
개인적인 의견으로는 0일로 계산하는게 맞다고 생각하는데  
문제에서 원하는 것은 1일로 계산하는 것이다.  
그래서 ```TIMESTAMPDIFF```값에 1을 더해줘야 한다.  
```sql
TIMESTAMPDIFF(DAY, START_DATE, END_DATE)+1
```  

구한 대여기간을 ```GROUP BY```를 사용해 ```CAR_ID``` 별로 평균 낸다.
```sql
GROUP BY CAR_ID
```  
```HAVING```을 사용해 평균 대여 기간이 7일 이상인 것만 선택한다.  
```sql
HAVING AVERAGE_DURATION >= 7
```
```ORDER BY```로 ```AVERAGE_DURATION_ID```과 ```CAR_ID```기준으로 내림차순 ```DESC``` 정렬한다.  
```sql
ORDER BY AVERAGE_DURATION DESC, CAR_ID DESC
```
