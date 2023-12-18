---
published: True
title:  "[SQL Server] Transaction count after EXECUTE indicates a mismatching number of BEGIN and COMMIT statements. Previous count = 0, current count = 1."
categories: [SQL_Server]
tag: [SQL, MSSQL, SQLServer]
---

## 오류 내용

```Transaction count after EXECUTE indicates a mismatching number of BEGIN and COMMIT statements. Previous count = 0, current count = 1.```  

## 원인  

SP 실행 중 오류로 인해 트랜잭션이 COMMIT 혹은 ROLLBACK으로 마무리되지 못했을 때 발생한다.  

## 해결방법  

1. TRY/CATCH 로 잡을 수 있는 에러의 경우는 TRY/CATCH 를 사용한다.  
    ```sql
    CREATE PROCEDURE SP_TEST
    AS
    BEGIN TRY
        BEGIN TRANSACTION
            SELECT  1 / 0 -- 0 으로 나누기
        COMMIT TRANSACTION
    END TRY
    BEGIN CATCH
        ROLLBACK TRANSACTION -- 롤백
    END CATCH
    ```  

2. ```SET XACT_ABORT ON``` 사용  
    ```SET XACT_ABORT ON``` 을 사용시 SP가 에러를 일으키면 트랜잭션을 자동으로 롤백한다.
    ```sql
    CREATE PROCEDURE SP_TEST
    AS
    SET XACT_ABORT ON -- 에러 발생시 트랜잭션 롤백

    BEGIN TRANSACTION

        INSERT INTO dbo.ExistTable VALUES (N'data') -- 다음 라인 에러로 인서트 롤백 됨
        SELECT * FROM dbo.NonexistTable -- 없는 테이블

    COMMIT TRANSACTION
    ```  

### 참고

<https://www.sysnet.pe.kr/2/0/12217>  
<https://learn.microsoft.com/ko-kr/sql/t-sql/statements/set-xact-abort-transact-sql?view=sql-server-ver16>  



