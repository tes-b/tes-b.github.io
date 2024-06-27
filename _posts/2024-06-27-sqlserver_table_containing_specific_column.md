---
published: True
title:  "[SQL Server] 특정 컬럼이 포함된 테이블 찾기"
categories: [SQL_Server]
tag: [SQL, MSSQL, SQLServer]
---

## 컬럼과 테이블 찾기

1. 특정 컬럼이 포함된 테이블 찾기  
    ```sql
    SELECT object_name(object_id) as TableName,* 
    FROM sys.columns 
    WHERE [name] = '찾을컬럼이름'
    ORDER BY TableName
    ```  

2. 전체 테이블과 컬럼 조회
    ```sql
    SELECT T.name AS table_name, C.name AS column_name
    FROM sys.tables AS T
    INNER JOIN sys.columns AS C
    ON T.object_id = C.object_id
    ```
