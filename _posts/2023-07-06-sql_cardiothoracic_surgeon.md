---
published: True
title:  "[Programmers, MySQL, Lv.1] 흉부외과 또는 일반외과 의사 목록 출력하기"
categories: [Programmers, MySQL]
tag: [Programmers, MySQL, Database, SQL]
---


## 문제

> DOCTOR 테이블에서 진료과가 흉부외과(CS)이거나 일반외과(GS)인 의사의 이름, 의사ID, 진료과, 고용일자를 조회하는 SQL문을 작성해주세요. 이때 결과는 고용일자를 기준으로 내림차순 정렬하고, 고용일자가 같다면 이름을 기준으로 오름차순 정렬해주세요.

## 쿼리

```sql
SELECT dr_name, dr_id, mcdp_cd, DATE_FORMAT(hire_ymd, '%Y-%m-%d') AS hire_ymd
FROM doctor
WHERE mcdp_cd IN ('CS','GS')
ORDER BY hire_ymd DESC, dr_name ASC
```

## 풀이

1. ```mcdp_cd```가 ```CS```거나 ```GS```인 것만 뽑는다.  
```sql
WHERE mcdp_cd IN ('CS','GS')
```

2. ```DATE_FORMAT``` 으로 ```hire_ymd```를    
주어진 형식에 맞게 바꾼다.  
```sql
SELECT dr_name, dr_id, mcdp_cd, DATE_FORMAT(hire_ymd, '%Y-%m-%d') AS hire_ymd
```

3. ```hire_ymd```을 내림차순으로  
```dr_name```을 오름차순으로 정렬한다.
```sql
ORDER BY hire_ymd DESC, dr_name ASC
```