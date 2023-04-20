---
published: false
title:  "데이터 사이언스 용어 정리"
categories: [DataScience]
tag: [DataScience]
---


Database  
전자적으로 저장되고 사용되는 관련있는 데이터들의 조직화된 집합  
  
DBMS  
DataBase Management Systems 의 약자  
사용자에게 DB를 정의하고 만들고 관리하는 기능을 제공하는 소프트웨어 시스템  
대표적으로 Oracle Database, MySQL, MS SQL, PostgreSQL 등이 있다.  

DB를 정의하다 보면 부가적인 데이터가 발생한다.  

부가적인 데이터를 metadata 라고 부른다.  

metadata  
database를 정의하거나 기술하는 data  
catalog라고도 부름  
예) 데이터 유형, 구조, 제걍조건, 보안, 저장, 인덱스, 사용자 그룹 등등  
metadata 또한 DBMS를 통해 저장/관리된다.  

database system  
database + DBMS + 연관된 applications  
줄여서 databse라고도 부름

data model
DB의 구조를 기술하는데사용될 수 있는 개념들이 모인 집합.  
DB 구조를 추상화해서 표현할 수 있는 수단을 제공한다.  
data model은 여러 종류가 있으며 추상화 수준과 DB 구조화 방식이 조금씩 다르다.  
DB에서 읽고 쓰기 위한 기본적인 동작들도 포함한다.
DB 구조 : 데이터 유형, 데이터관계, 제약사항 등등

data model 분류
- conceptual (or high-level) data models
- logical (or representational) data models
- physical (or low-level) data models

conceptual data models 
- 일반 사용자들이 쉽게 이해할 수 있는 개념들로 이뤄진 모델  
- 추상화 수준이 가장 높음.  
- 비즈니스 요구 사항을 추상화하여 기술할 때 사용  
- 대표적인 모델로 entity-reslationship model이 있음

logical data models
- 이해하기 어렵지 않으면서도 디테일하게 DB를 구조화 할 수 있는 개념들을 제공
- 데이터가 컴퓨터에 저장 될 때의 구조와 크게 다르지 않게 DB 구조화를 가능하게 함
- 특정 DBMS나 stroage 에 종속되지 않는 수준에서 DB를 구조화 할 수 있는 모델
- 대표적인 모델  
    1. relational data model
    2. object data model
    3. object-relational data model
     
physical data model 
- 컴퓨터에 데이터가 어떻게 파일 형태로 저장되는지를 기술할 수 있는 수단을 제공
- data fromat, data orderings, access path 등등
- access path : 데이터 검색을 빠르게 하기 위한 구조체, e.g) index

database schema
- data model을 바탕으로 database의 구조를 기술한것
- schema는 database 를 설계할 때 정해지며 한번 정해진 후에는 자주 바뀌지 않는다

database state
- database에 있는 실제 데이터는 꽤 자주 바뀔 수 있다
- 특정 시점에 database에 있는 데이터를 database state 혹은 snapshot이라고 한다.
- 혹은 database에 있는 현재 instances의 집합이라고도 한다.

three-schema architecture
- database system을 구축하는 architecture 중의 하나
- user application으로 부터 물리적인 database를 분리시키는 목적
- 세 가지 level이 존재하며 각각의 level마다 schema가 정의되어 있다.
- levels
    1. external schemaa at external level
    2. conceptual schemas at conceptual level
    3. internal schemas at internal level

internal schema 
- 물리적으로 데이터가 어떻게 저장되는지 physical data model을 통해 표현
- data storage, data structure, access path 등등 실체가 있는 내용 기술

external schema
- external views user views 라고도 불림
- 특정 유저들이 필요로 하는 데이터만 표현
- 그 외 알려줄 필요가 없는 데이터는 숨김
- logical data model을 통해 표현

database system의 초창기 아키텍쳐는 internal schema 와 external schema 만 존재.  
각각의 유저마다 필요로 하는 데이터들이 달라지다보니 internal level에서도 중복되는 데이터가 생김.  
필요에 맞춰 데이터를 제공하려 하다보니 같은 데이터를 포함함에도 조금씩다른 internal schema 들이 여러개 생김  
중복된 데이터들을 포함하는 여러 스키마들이 생기면서 관리가 힘들어지고 데이터 불일치도 발생  
이 문제를 해결하기 위해 나온것이 conceptual schema

conceptual schema 
- 전체 database에 대한 구조를 기술 
- 물리적인 저장 구조에 관한 내용은 숨김
- entities, data types, relationships, user operations, constraints에 집중
- logical data model을 통해 기술

three-schema architecture
- 각 레벨을 독립시켜서 어느 레벨에서의 변화가 상위 레벨에 영향을 주지 않기 위함
- 대부분의 DBMS가 three level을 완벽하게 혹은 명시적으로 나누지는 않음
- 데이터가 존재하는 곳은 internal level

안정적으로 데이터 베이스 시스템을 운영하기 위한 아키텍쳐
internal schema에 변경이 생겨도 그 변경으로 인해 conceptual schema도 바꿔줘야 할 필요가 없다.   
둘 사이의 mapping만 바꿔주면 된다.  
conceptual schema와 external schema도 동일하지만 상대적으로 구현이 더 어려운 편.

database language
data definition language (DDL)
- conceptual schema를 정의하기 위해 사용되는 언어
- internal schema까지 정의할수 있는 경우도 있음

storage definition language (SDL)
- internal schema를 정의하는용도로 사용되는 언어
- 요즘은 특히 relational DBMS에서는 SDL이 거의 없고 파라미터 등의 설정으로 대체됨

view definition language (VDL)
- external schemas를 정의하기 위해 사용되는 언더
- 대부분의 DBMS에서는 DDL이 VDL역할까지 수행

data manipulation language (DML)
- database에 있는 data를 활요하기 위한 언어
- data 추가, 삭제, 수정, 검색 등등의 기능을 제공하는 언더

통합된 언어
- 오늘날의 DBMS에는 DML, VDL, DDL이 따로 존재하기 보다는 통합된 언어로 존재
- 대표적인 예가 relational database language : SQL

### 내용 출처
https://www.youtube.com/watch?v=aL0XXc1yGPs