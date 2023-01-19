---
published: True
title:  "[MySQL] 데이터 베이스 내보내기(dump)/불러오기(import)"
categories: Database
tag: [SQL, MySQL, RDBMS, Database]
---

## 내보내기
- 전체 스키마
```
mysqldump -u[아이디] -p[패스워드] [데이터베이스명] > [저장할 경로와 파일명].sql
```
- 특정 테이블
```
mysqldump -u[아이디] -p[패스워드] [데이터베이스명] [테이블명] > [저장할 경로와 파일명].sql
```

예시)  
아이디 : ```id```  
패스워드 : ```1234```  
데이터베이스명 : ```database_name```  
테이블명 : ```table_name```  
저장할 경로 : ```/root/backup```  
파일명 : ```data```

- 전체 스키마  
```
mysqldump -uid -p1234 database_name > /root/backup/data.sql
```
- 특정 테이블  
```
mysqldump -uid -p1234 database_name table_name > /root/backup/data.sql
```


## 불러오기

불러오기는 내보내기와 꺽쇠의 방향이 반대이다.  
데이터베이스는 미리 생성해둬야 한다.

- 전체 스키마
```
mysql -u[아이디] -p[패스워드] [데이터베이스명] < [불러올 경로와 파일명].sql
```
- 특정 테이블
```
mysql -u[아이디] -p[패스워드] [데이터베이스명]=[테이블명] < [저장할 경로와 파일명].sql
```

예시)  
아이디 : ```id```  
패스워드 : ```1234```  
데이터베이스명 : ```database_name```  
테이블명 : ```table_name```  
저장할 경로 : ```/root/backup```  
파일명 : ```data```


```
// 데이터베이스 먼저 생성
CREATE DATABASE sample_data;
```
- 전체 스키마  
```
mysqldump -uid -p1234 database_name < /root/backup/data.sql
```
- 특정 테이블  
```
mysqldump -uid -p1234 database_name=table_name < /root/backup/data.sql
```

### 참고
https://cheershennah.tistory.com/173