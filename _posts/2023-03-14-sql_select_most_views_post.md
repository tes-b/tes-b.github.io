---
published: True
title:  "[Programmers, MySQL, Lv.3] 조회수가 가장 많은 중고거래 게시판의 첨부파일 조회하기"
categories: CodingTests
tag: [Programmers, MySQL, Database, SQL]
---

## 문제

> USED_GOODS_BOARD와 USED_GOODS_FILE 테이블에서 조회수가 가장 높은 중고거래 게시물에 대한 첨부파일 경로를 조회하는 SQL문을 작성해주세요. 첨부파일 경로는 FILE ID를 기준으로 내림차순 정렬해주세요. 기본적인 파일경로는 /home/grep/src/ 이며, 게시글 ID를 기준으로 디렉토리가 구분되고, 파일이름은 파일 ID, 파일 이름, 파일 확장자로 구성되도록 출력해주세요. 조회수가 가장 높은 게시물은 하나만 존재합니다.

## 해답

``` SQL
SELECT CONCAT("/home/grep/src/", B.BOARD_ID, "/", F.FILE_ID, F.FILE_NAME, F.FILE_EXT) AS FILE_PATH
FROM USED_GOODS_BOARD AS B
JOIN USED_GOODS_FILE AS F 
ON B.BOARD_ID = F.BOARD_ID
WHERE VIEWS = (SELECT MAX(VIEWS) FROM USED_GOODS_BOARD)
ORDER BY F.FILE_ID DESC;
```

## 풀이

```JOIN```으로 ```BOARD_ID```를 기준으로 두 테이블을 합친다.  
```MAX```로 ```VIEWS```항목이 가장 큰 게시물을 선택.  
```ORDER BY```로 ```FILE_ID```기준으로 내림차순 ```DESC``` 정렬한다.  
```CONCAT``` 으로 필요한 내용들을 붙여 파일 경로를 만든다.  