---
published: True
title:  "[MySQL, Ubuntu] 쓸때마다 까먹는 MySQL 명령어들(실행, 접속, 비밀번호 재설정)"
categories: Database
tag: [MySQL, ubuntu]
---


매번 검색하기 귀찮아서 정리해둠  

## 우분투 MySQL 시작, 재시작, 정지, 상태확인 방법  

|작업|우분투명령어|  
|---|---|  
|시작|service mysql start|
|정지|service mysql stop|
|재시작|service mysql restart|
|상태확인|service mysql status|

## 비밀번호 변경

### 로그인
```
mysql -u root -p
```

### 사용자 암호 설정
```sql
ALTER USER 'user-name'@'localhost' IDENTIFIED BY 'NEW_USER_PASSWORD';
FLUSH PRIVILEGES;
```
```ALTER USER``` 작동하지 않는 경우
```sql
UPDATE mysql.user SET authentication_string = PASSWORD('NEW_USER_PASSWORD')
WHERE User = 'user-name' AND Host = 'localhost';
FLUSH PRIVILEGES;
```

## 데이터베이스, 사용자, 테이블

### 데이터베이스 생성
```sql
CREATE DATABASE db_name default CHARACTER SET UTF8; 
```

```sql
GRANT ALL PRIVILEGES ON db_name.* TO db_user@localhost IDENTIFIED BY 'user';
EXIT;
mysql -u db_user -p
USE db_name;
```


### 참고

<https://zetawiki.com/wiki/%EB%A6%AC%EB%88%85%EC%8A%A4_MySQL_%EC%8B%9C%EC%9E%91,_%EC%A0%95%EC%A7%80,_%EC%9E%AC%EC%8B%9C%EC%9E%91,_%EC%83%81%ED%83%9C%ED%99%95%EC%9D%B8>

<https://kakaoapps.com/%EC%9A%B0%EB%B6%84%ED%88%AC-mysql-%EC%8B%9C%EC%9E%91-%EC%9E%AC%EC%8B%9C%EC%9E%91-%EC%A0%95%EC%A7%80-%EC%83%81%ED%83%9C%ED%99%95%EC%9D%B8-%EB%B0%A9%EB%B2%95/>


비밀번호 재설정
<https://jjeongil.tistory.com/1484>

db생성, 테이블 생성
<https://futurists.tistory.com/11>