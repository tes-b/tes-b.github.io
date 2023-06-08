---
published: True
title:  "[HackerRank, MySQL, Medium] The PADS"
categories: [HackerRank, MySQL]
tag: [Programmers, MySQL, Database, SQL]
---

## 문제

> 테이블에서 "이름(직업이니셜)"과 "There are a total of (직업 별 인원수) (직업)s." 를 뽑아내 정렬해야 한다.  


예시  
```
Ashely(P)
Christeen(P)
Jane(A)
Jenny(D)
Julia(A)
Ketty(P)
Maria(A)
Meera(S)
Priya(S)
Samantha(D)
There are a total of 2 doctors.
There are a total of 2 singers.
There are a total of 3 actors.
There are a total of 3 professors.
```

## 쿼리

```sql
(SELECT CONCAT(name,"(",LEFT(occupation,1),")") AS list
FROM occupations 
ORDER BY name)

UNION ALL

(SELECT CONCAT("There are a total of ",COUNT(name)," ",LOWER(occupation),"s.") AS list
FROM occupations
GROUP BY occupation
ORDER BY COUNT(name) ASC, occupation ASC)

ORDER BY list
;
```

## 풀이

두개의 쿼리문이 필요하고 ```UNION ALL```을 이용해서 합치는 문제이다.  
쿼리에서 정렬을 했더라도 ```UNION```을 이용하면 정렬이 깨지기 때문에  
마지막에 정렬을 다시 해줘야 한다.

1. 첫번째 쿼리는 ```CONCAT```으로 ```name```과  ```occupation```을 조건에 맞게 붙여준다.  
```LEFT```를 사용해서 ```occupation```의 첫글자만 뽑아내면 된다.  
컬럼이름을 붙여서 두번째 쿼리와 컬럼 이름을 통일시킨다.  
```sql
(SELECT CONCAT(name,"(",LEFT(occupation,1),")") AS list
FROM occupations 
ORDER BY name)
```

2. 두번째 쿼리는 ```occupation```을 ```GROUP BY```로 묶어주고  
첫번째 쿼리처럼 ```CONCAT```으로 ```COUNT(name)```과  ```occupation```을 조건에 맞게 붙여준다.   
```ORDER BY```로 ```COUNT(name)```, ```occupation``` 순서로 정렬한다.  
컬럼 이름은 첫번째와 동일하게 한다. 
```sql
(SELECT CONCAT("There are a total of ",COUNT(name)," ",LOWER(occupation),"s.") AS list
FROM occupations
GROUP BY occupation
ORDER BY COUNT(name) ASC, occupation ASC)
```

3. ```UNION ALL``` 로 두 쿼리를 이어붙이고.
```ORDER BY```로 다시 정렬해준다.    
```sql
(SELECT ... AS list FROM ... )
UNION ALL
(SELECT ... AS list FROM ... )
ORDER BY list
```