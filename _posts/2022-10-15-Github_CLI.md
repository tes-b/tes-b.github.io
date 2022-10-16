---
published: false
title:  "Github Cli 사용하기 (fork, pull request)"
categories: etc
tag: [GitHub, GitHub CLI, fork, pull request]
---

## GitHub CLI
깃허브를 CLI환경에서 사용할 수 있다고 한다.  
요즘 CLI환경에 익숙해지기 위해  
여러 시도들을 하는 중이라 그냥 넘어갈 수 없었다.

## 설치
https://cli.github.com/ 에서 설치 할 수 있다.    
![image0](/images/2022-10-15-Github_CLI_0.png)
![image1](/images/2022-10-15-Github_CLI_1.png)
설치 완료!

## 로그인

터미널에서 아래 명령을 넣으면 로그인을 할 수 있다.
> gh auth login

![image2](/images/2022-10-15-Github_CLI_2.png)  
몇가지 질문들이 있는데 아래처럼 선택해주고  
마지막 질문에 'Login with a web browser'를 선택!  

인증 코드를 알려주는데 엔터를 치면  
아래처럼 코드를 입력하는 페이지가 나온다.  
![image3](/images/2022-10-15-Github_CLI_3.png)  

코드를 넣으면 인증이 완료된다.  
![image4](/images/2022-10-15-Github_CLI_4.png)  

## pull request test
> gh pr create 명령어로 pull request 할 수 있다.

아래 스샷은 윈도우에서 gh cli로 pull request를 해본 모습이다.  
![image5](/images/2022-10-15-Github_CLI_5.png)  


### 메뉴얼  
https://cli.github.com/manual/index

### 문제  
윈도우에서 gitbash 터미널로 사용시 동작이 안된다.  
몇몇 명령들은 질문에 대해 답변을 선택해야 하는데  
이 동작이 gitbash에서는 안되는 듯??  
anaconda prompt에서는 정상적으로 동작했다.
