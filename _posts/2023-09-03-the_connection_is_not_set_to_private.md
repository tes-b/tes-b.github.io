---
published: True
title:  "[.NET] 크롬 닷넷 웹 '연결이 비공개로 설정되어 있지 않습니다' 해결방법"
categories: [.NET]
tag: [.NET, Chrome]
---

최근 ASP.NET을 쓸 일이 생겨 닷넷 공부도 할 겸  
홈페이지 프레임워크를 장고에서 .NET Core로 바꾸려고 한다.  
프로젝트 시작부터 문제가 생겨 기록을 남긴다.  

## 문제 
비주얼 스튜디오에서 .NET Core MVC 프로젝트를 만들고 디버그를 실행하니  
다음과 같은 문제로 페이지가 보이지 않았다.  

![Image1](/images/2023-09-03-the_connection_is_not_set_to_private_0.jpg)*이미지는 같은 에러를 웹에서 가져온 것이다.*   

## 해결방법

이 문제는 브라우저 보안 세팅을 바꿔주면 된다.  
크롬의 경우 ```chrome://flags/```로 들어간뒤  
```Allow invalid certificates for resources loaded from localhost``` 플래그를 Enabled로 바꿔주면 된다.  

![Image1](/images/2023-09-03-the_connection_is_not_set_to_private_1.png)

### 출처  

<https://stackoverflow.com/questions/44066709/your-connection-is-not-private-neterr-cert-common-name-invalid>  
