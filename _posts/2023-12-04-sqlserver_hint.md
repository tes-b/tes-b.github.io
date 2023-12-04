---
published: True
title:  "[SQL Server] 힌트 사용 방법"
categories: [SQL_Server]
tag: [SQL, MSSQL, SQLServer]
---

## 테이블 힌트 사용방법

```sql
FROM t (TABLOCK)
FROM t WITH (TABLOCK)
```

힌트를 다른 옵션과 함께 지정하는 경우 다음과 같이 힌트를 WITH 키워드로 지정해야 한다.
```sql
FROM t WITH (TABLOCK, INDEX(myindex))
```


## 인덱스 힌트

인덱스 이름으로 지정
```sql
SELECT * FROM t WITH(INDEX(인덱스이름))
```

인덱스 번호로 지정
```sql
SELECT * FROM t WITH(INDEX(0)) -- 0번은 풀스캔
SELECT * FROM t WITH(INDEX(1))
SELECT * FROM t WITH(INDEX(2))
```

## 테이블 락

테이블 잠금을 설정한다. 잠금 유형은 실행 중인 문에 따라 달라진다.
```sql
SELECT * FROM t WITH(TABLOCK)
``` 

베타적 잠금(Exclusive Lock) 설정
```sql
SELECT * FROM t WITH(TABLOCKX)
```

테이블 락 없이 조회
```sql
SELECT * FROM t WITH(NOLOCK)
``` 

## Join 힌트

{LOOP | MERGE | HASH } JOIN : Join 힌트

HASH 
```sql
SELECT * 
FROM t1 AS A
LEFT OUTER HASH JOIN t2 AS B
ON A.xx= B.xx
```

LOOP
```sql
SELECT * 
FROM t1 AS A
INNER LOOP JOIN t2 AS B
ON A.xx= B.xx
```

MERGE
```sql
SELECT * 
FROM t1 AS A
INNER MERGE JOIN t2 AS B
ON A.xx= B.xx
```

## 쿼리 힌트 

```OPTION (HINT)``` 형식으로 사용한다.
```sql
SELECT * 
FROM t1 AS A 
JOIN t2 AS B 
ON A.xx= B.xx
OPTION (MERGE JOIN);
```

쿼리 힌트에서의 조인 형식은 아래 예시처럼 두 가지 이상을 지정할 수도 있다.  
이 경우 옵티마이저에 의해 가장 비용이 낮은 것이 사용된다.  

만약 조인 힌트와 쿼리 힌트에서 둘 다 조인 형식을 지정한다면 조인 힌트의 것이 우선권을 가진다.
```sql
SELECT *
FROM Sales.Customer AS c
INNER JOIN Sales.CustomerAddress AS ca ON c.CustomerID = ca.CustomerID
WHERE TerritoryID = 5
OPTION (MERGE JOIN, LOOP JOIN);
```  

{HASH | ORDER } GROUP : 집계 형식  

쿼리의 GROUP BY, DISTINCT 또는 COMPUTE 절에 지정된 집계에서 해시나 정렬을 사용하도록 지정.
 
```sql
SELECT *
FROM t 
GROUP BY xx, yy
ORDER BY xx, yy
OPTION (HASH GROUP) ;
```


OPTION(FORCE ORDER)  
from 절에 나열된 순서대로 JOIN 한다.
```sql
SELECT * 
FROM t AS A, t2 AS B 
WHERE A.xx= B.xx
OPTION (FORCE ORDER);
```

### 출처

<https://devjino.tistory.com/95>  
<https://learn.microsoft.com/ko-kr/sql/t-sql/queries/hints-transact-sql-query?view=sql-server-ver16>  
<https://learn.microsoft.com/ko-kr/sql/t-sql/queries/hints-transact-sql-join?view=sql-server-ver16>


