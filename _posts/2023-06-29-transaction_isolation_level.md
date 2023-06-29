---
published: True
title:  "[Database] 트랜잭션 격리수준 (isolation level)"
categories: [Database]
tag: [Database, DataScience, transaction, DBMS, MSSQL]
---


## READ UNCOMMITTED

다른 트랜잭션이 변경 중인 데이터를 읽을 수 있는 격리 수준.  
다른 트랜잭션에 의해 수정되었지만 아직 커밋되지 않은 데이터를 읽을 수 있다.  
해당 트랜잭션이 롤백될 경우 잘못된 데이터를 읽는게 되어 일관성에 문제가 생길 수 있는데 이를 Dirty Read라고 한다.   
동시성이 뛰어나기 때문에 일관성에 민감한 데이터가 아닐경우 사용하게 된다.  

## READ COMMITTED

기본 격리수준 이다.  
다른 트랜잭션이 데이터 변경 작업을 완료(커밋이나 롤백) 할 떄까지 기다려야 한다.

## REPEATABLE READ

트랜잭션을 종료할 때까지 결과 집합의 행을 다른 트랜잭션이 변경할 수 없다.  
다만 데이터 추가가 가능하기 때문에 한 트랜잭션 내에서 동일한 데이터를 두번 읽는 경우  
둘 사이에 데이터 추가가 발생한다면 동일한 데이터를 읽었지만 두번째 읽기에서는 추가된 데이터가 포함된다.  
이를 Phantom Read라고 하는데 데이터베이스 제품에 따라 같은 REPEATABLE READ 격리수준이라도  
해당 문제가 발생하지 않는 것도 있다고 한다.  

## SERIALIZABLE

가장 강력한 격리 수준이다.  
트랜잭션이 읽은 결과 집합의 범위를 잠가서 다른 트랜잭션이 그 범위의 데이터를 변경하지 못하게 한다.  
Phantom Read 가 발생하지 않지만 동시성이 떨어진다.  

## READ COMMITTED SNAPSHOT

READ_COMMITTED_SNAPSHOT 속성을 ON으로 변경하면  
다른 트랜잭션이 데이터를 변경하는 동안  
잠금으로 대기하지 않고 변경 전 데이터를 즉시 읽을 수 있다.  
변경전 데이터를 스냅샷 하여 읽을 수 있도록 하는 것.  
높은 동시성을 기대 할 수 있다.  

## SNAPSHOT

ALLOW_SNAPSHOT_ISOLATION 속성을 ON으로 변경.  
트랜잭션 내에서 계속해서 같은 데이터를 읽을 수 있다.  
이때 다른 세션은 데이터를 자유롭게 수정하거나 삭제할 수 있다.  

## 트랜잭션 격리수준 특성 비교

|격리수준|Dirty Read|Nonrepeatable Read|Phantom Read|
| ----------------------- | ---------- | ------------------ | ------------ |
| READ UNCOMMITTED        | Yes        | Yes                | Yes          |
| READ COMMITTED          | No         | Yes                | Yes          |
| REPEATABLE READ         | No         | No                 | Yes          |
| SERIALIZABLE            | No         | No                 | No           |
| SNAPSHOT                | No         | No                 | No           |
| READ COMMITTED SNAPSHOT | No         | Yes                | Yes          |

## 트랜잭션 격리 수준 관련 테이블 힌트

|힌트|설명|  
|-----------------|---------------------------- |
| READCOMMITTED     | READ COMMITETED 격리수준 사용|
| READCOMMITTEDLOCK | 해당 데이터베이스가 READ COMMITTED SNAPSHOT이 활성화된 상태더라도 <br>  READ COMMITTED 트랜잭션 격리 수준을 상태에서 잠금을 검 |
| REDUNCOMMITTED    | READ UNCOMMITTED 격리 수준 사용|
| REPEATABLEREAD    | REPEATABLE READ 격리 수준 사용|
| SERIALAZABLE      | SERIALIZABLE 격리 수준 사용|