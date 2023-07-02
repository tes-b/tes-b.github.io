---
published: True
title:  "[Programmers, MySQL, Lv.4] 취소되지 않은 진료 예약 조회하기"
categories: [Programmers, MySQL]
tag: [Programmers, MySQL, Database, SQL]
---


## 문제

> PATIENT, DOCTOR 그리고 APPOINTMENT 테이블에서 2022년 4월 13일 취소되지 않은 흉부외과(CS) 진료 예약 내역을 조회하는 SQL문을 작성해주세요. 진료예약번호, 환자이름, 환자번호, 진료과코드, 의사이름, 진료예약일시 항목이 출력되도록 작성해주세요. 결과는 진료예약일시를 기준으로 오름차순 정렬해주세요.  

## 쿼리

```sql
SELECT a.apnt_no, p.pt_name, a.pt_no, a.mcdp_cd, d.dr_name, a.apnt_ymd
FROM appointment AS a
    JOIN doctor AS d
    ON a.mddr_id = d.dr_id
    JOIN patient AS p
    ON a.pt_no = p.pt_no
WHERE DATE_FORMAT(apnt_ymd, '%Y-%m-%d') = '2022-04-13'
AND apnt_cncl_yn = 'N'
AND a.mcdp_cd = 'CS'
ORDER BY a.apnt_ymd ASC
```

## 풀이

appointment 테이블에서 조건에 맞는 예약만 뽑은 후  
나머지 항목은 테이블 조인으로 채워넣으면 된다. 

1. 먼저 ```appointment```테이블에서 조건에 맞는 항목을 뽑는다.  

```sql
SELECT *
FROM appointment AS a
WHERE DATE_FORMAT(apnt_ymd, '%Y-%m-%d') = '2022-04-13'
AND apnt_cncl_yn = 'N'
AND a.mcdp_cd = 'CS'
```

2. ```doctor``` 테이블을 ```dr_id```로  
```patient``` 테이블을 ```pt_no```로 조인한다.
```sql
SELECT *
FROM appointment AS a
    JOIN doctor AS d
    ON a.mddr_id = d.dr_id
    JOIN patient AS p
    ON a.pt_no = p.pt_no
WHERE DATE_FORMAT(apnt_ymd, '%Y-%m-%d') = '2022-04-13'
AND apnt_cncl_yn = 'N'
AND a.mcdp_cd = 'CS'
```

3. 필요한 컬럼을 ```SELECT```한다.  
```sql
SELECT a.apnt_no, p.pt_name, a.pt_no, a.mcdp_cd, d.dr_name, a.apnt_ymd
FROM appointment AS a
    JOIN doctor AS d
    ON a.mddr_id = d.dr_id
    JOIN patient AS p
    ON a.pt_no = p.pt_no
WHERE DATE_FORMAT(apnt_ymd, '%Y-%m-%d') = '2022-04-13'
AND apnt_cncl_yn = 'N'
AND a.mcdp_cd = 'CS'
```

4. ```apnt_ymd```를 오름차순으로 정렬한다.
```sql
ORDER BY a.apnt_ymd ASC
```