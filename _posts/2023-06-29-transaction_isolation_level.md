---
published: True
title:  "[SQL Server] 트랜잭션 격리수준 (Transaction Isolation Level)"
categories: [SQL_Server]
tag: [SQL, MSSQL, SQLServer]
---
## 트랜잭션 격리수준

### READ UNCOMMITTED

다른 트랜잭션이 변경 중인 커밋되지 않은 데이터를 읽을 수 있는 격리 수준.

변경중인 트랜잭션이 롤백된 후 다시 읽으면 변경전 데이터를 읽게되어 일관성에 문제가 될 수 있다.(Dirty Read)

격리 수준 중 제한이 가장 적어 동시성이 뛰어나다.

## READ COMMITTED

SQL Server의 기본값이다.

다른 트랜잭션에 의해 수정되었지만 커밋되지 않은 데이터를 읽을 수 없다.

### REPEATABLE READ

트랜잭션을 종료할 때까지 결과 집합의 행을 다른 트랜잭션이 변경할 수 없다.

데이터 추가가 가능하기 때문에 데이터를 다시 읽을 때 새로운 행이 포함될 수 있다.(Phantom Read)

### SERIALIZABLE

트랜잭션이 읽은 결과 집합의 범위를 잠가서 다른 트랜잭션이 그 범위의 데이터를 변경하지 못하게 한다.

가장 강력한 격리 수준으로 Dirty Read, Nonrepeatable Read, Phantom Read 가 발생하지 않지만 동시성이 떨어진다.

### READ COMMITTED SNAPSHOT

READ_COMMITTED_SNAPSHOT 속성을 ON으로 변경하면

다른 트랜잭션이 데이터를 변경하는 동안, 잠금으로 대기하지 않고 변경 전 데이터를 즉시 읽을 수 있다.

변경전 데이터를 스냅샷해두고 읽을 수 있도록 하는것.

높은 동시성을 기대 할 수 있다.

### SNAPSHOT

ALLOW_SNAPSHOT_ISOLATION 속성을 ON으로 변경

트랜잭션 내에서 계속해서 같은 데이터를 읽을 수 있다.

이때 다른 세션은 데이터를 자유롭게 수정하거나 삭제할 수 있다.

## 트랜잭션 격리수준 특성 비교

| 격리수준                | Dirty Read | Nonrepeatable Read | Phantom Read |
| ----------------------- | ---------- | ------------------ | ------------ |
| READ UNCOMMITTED        | Yes        | Yes                | Yes          |
| READ COMMITTED          | No         | Yes                | Yes          |
| REPEATABLE READ         | No         | No                 | Yes          |
| SERIALIZABLE            | No         | No                 | No           |
| SNAPSHOT                | No         | No                 | No           |
| READ COMMITTED SNAPSHOT | No         | Yes                | Yes          |
