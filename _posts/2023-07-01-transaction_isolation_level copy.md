---
published: True
title:  "[Database] 제약(Contraints)"
categories: [Database]
tag: [Database, DataScience, transaction, DBMS, MSSQL]
---

## PRIMARY KEY 제약

테이블 당 한번만 설정 가능.  
NULL값 불가.  
FOREIGN KEY 참조 대상열 가능.  

```sql
CREATE TABLE dbo.Employee (
    EmpID char(5) PRIMARY KEY
    EmpName nvarchar(10) NOT NULL
)
```

확인
```sql
SELECT name
FROM sys.key_constraints
WHERE parent_object_id = OBJECT_ID('dbo.Employee', 'U') ANd type = 'PK'
```

삭제
```sql
ALTER TABLE dbo.Employee
DROP CONSTRAINT PK__Employee__AF2DBA7936DFF650
```

추가
```sql
ALTER TABLE dbo.Employee
ADD PRIMARY KEY (EmpID)
```

## UNIQUE 제약

NULL값이 가능(NULL 끼리는 중복으로 간주하지 않음)   
고유한 비클러스터형 인덱스 만들어짐.  
FOREIGN KEY 제약의 참조 대상열 가능.  

```sql
CREATE TABLE dbo.Employee (
    EmpID char(5) PRIMARY KEY,
    EmpName nvarchar(10) NOT NULL,
    EMail varchar(60) UNIQUE NOT NULL
)
```

확인
```sql
SELECT name
FROM sys.key_constraints
WHERE parent_object_id = OBJECT_ID('dbo.Employee', 'U') ANd type = 'UQ'
```

삭제
```sql
ALTER TABLE dbo.Employee
DROP CONSTRAINT UQ__Employee__AF2DBA7936DFF650
```

추가
```sql
ALTER TABLE dbo.Employee
ADD UNIQUE (EMail)
```

## DEFAULT 제약

INSERT 문 실행시 특정 열에 기본값이 저장되게 할 수 있다.
```sql
CREATE TABLE dbo.Vacation (
    VacationID int IDENTITY PRIMARY KEY,
    EmpID char(5) NOT NULL,
    BeginDate date NOT NULL,
    EndDate date NOT NULL,
    Reason nvarchar(50) DEFAULT N'개인사유',
    Duration AS (DATEDIFF(day, BeginDate, EndDate) + 1)
)
```

확인
```sql
SELECT name
FROM sys.default_constraints
WHERE parent_object_id = OBJECT_ID('dbo.Vacation', 'U') ANd type = 'D'
```

삭제
```sql
ALTER TABLE dbo.Employee
DROP CONSTRAINT DF__Vacation__Reason__31EC6D26
```

추가
```sql
ALTER TABLE dbo.Vacation
ADD DEFAULT N'개인사유' FOR Reason
```

## CHECK 제약

설정된 조건을 판단해서 조건에 맞지 않으면 입력을 거부한다.  

예시)  
EndDate가 BeginDate보다 크거나 같아야 한다.
```sql
CREATE TABLE dbo.Vacation (
    VacationID int IDENTITY PRIMARY KEY,
    EmpID char(5) NOT NULL,
    BeginDate date NOT NULL,
    EndDate date NOT NULL,
    Reason nvarchar(50) DEFAULT N'개인사유',
    Duration AS (DATEDIFF(day, BeginDate, EndDate) + 1),
    CHECK (EndDate >= BeginDate)
)
```

확인
```sql
SELECT name
FROM sys.check_constraints
WHERE parent_object_id = OBJECT_ID('dbo.Vacation', 'U') ANd type = 'C'
```

삭제
```sql
ALTER TABLE dbo.Vacation
DROP CONSTRAINT CK__Vacation__35BCFE0A
```

추가
```sql
ALTER TABLE dbo.Vacation
ADD CHECK (EndDate >= BeginDate)
```

## FOREGIN KEY 제약

같은 테이블이나 다른 테이블과의 참조 무결성을 유지.  
중복값이 없는 열만 참조 가능(PK, UQ).  
NULL 값 가능.  
NULL 값은 참조 대상이 되지 않음.  

```sql
CREATE TABLE dbo.Vacation (
    VacationID int IDENTITY PRIMARY KEY,
    EmpID char(5) REFERENCES dbo.Employee(EmpID),
    BeginDate date NOT NULL,
    EndDate date NOT NULL,
    Reason nvarchar(50) DEFAULT N'개인사유',
    Duration AS (DATEDIFF(day, BeginDate, EndDate) + 1),
    CHECK (EndDate >= BeginDate)
)
```

확인
```sql
SELECT name
FROM sys.foreign_keys
WHERE parent_object_id = OBJECT_ID('dbo.Vacation', 'U') ANd type = 'F'
```

삭제
```sql
ALTER TABLE dbo.Vacation
DROP CONSTRAINT FK__Vacation__EmpID__38996AB5
```

추가
```sql
ALTER TABLE dbo.Vacation
ADD FOREIGN KEY (EmpID) REFERENCES dbo.Employee(EmpID)
```