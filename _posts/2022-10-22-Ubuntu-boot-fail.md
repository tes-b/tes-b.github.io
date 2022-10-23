---
published: false
title:  "[ubuntu] 우분투 부팅 멈춤 현상 해결방법"
categories: ubuntu
tag: [ubuntu]
---

내 컴퓨터 사양 명시

## 우분투 부팅 실패

![image0](/images/2022-10-22-Ubuntu-boot-fail_0.png)  
우분투를 처음 설치하고 부팅을 하니 위의 화면에서  
멈추는 현상이 발생했다.  

처음부터 호락호락하지 않은 우분투...

## 해결 방법

에러 메세지로 구글링을 하니 해결한 분이 있어서 글을 참고했다.  
https://yskim0.github.io/troubleshooting/2021/01/26/Ubuntu-clean-files-blocks-trouble/

위의 블로그를 따라하니 일단 부팅까지는 성공!  

![image1](/images/2022-10-22-Ubuntu-boot-fail_1.png)  
![image2](/images/2022-10-22-Ubuntu-boot-fail_2.png)  
![image3](/images/2022-10-22-Ubuntu-boot-fail_3.png)  

> sudo apt-get purge nvidia*

드라이버 삭제 명령

> dpkg was interrupted, you must manually run 'sudo dpkg --configure -a' to correct the problem

위의 에러가 발생 할 경우
> sudo dpkg --configure -a 


그러나 그냥 따라 하니 부팅이 되었을 뿐  
정작 저 문제가 왜 발생하는지는 알지 못했는데  
그래픽 드라이버를 설치하는 과정에서 해당 문제가 다시 발생했다!!  

## 문제의 원인

결론부터 말하면 그래픽 드라이버의 충돌이었다.

> ubuntu-drivers devices 

위의 커멘드를 입력하면 설치가능한 드라이버 목록이 나온다.  

![image1](/images/2022-10-22-Ubuntu-boot-fail_1.png)  

여기서 recommeded라 써있는 515 버전 드라이버를 설치했는데  
이게 내 랩탑과 맞지 않았던 것 같다.  

그리고 위에서 소개한 블로그의 해결 방법은 그래픽 드라이버를 


> sudo ubuntu-drivers autoinstall

그리고 위의 명령어로 추천 드라이버를 설치 할 수 있는데  
이 드라이버가 

우분투 부팅시 화면에서 멈추거나 검은 화면이 나는 경우   
그래픽 드라이버의 문제

새로 설치시 자동으로 드라이버 설치하면 문제생길 수 있음

지우고 다시 설치하는 방법

그래픽 드라이버 찾아서 설치하기