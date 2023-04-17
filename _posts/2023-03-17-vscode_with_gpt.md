---
published: True
title:  "[AICodeHelper] VSCode에 chatGPT 연동하기"
categories: [VSCode]
tag: [chatGPT, VSCode, AICodeHelper]
---

VSCode에 chatGPT를 연동해서 쓰는 익스텐션을 소개하려 한다.  
따로 GPT페이지에서 질문하지 않아도 VSCode에서 바로 사용할 수 있어 편리하다.  

## AICodeHelper
익스텐션에서 AICodeHelper를 검색하면 로봇 아이콘의 익스텐션이 나오는데 설치해준다.  
<https://marketplace.visualstudio.com/items?itemName=Kimseungtae.aicodehelper>

![image0](/images/2023-03-17-vscode_with_gpt_0.png)  

설치 후 아래 링크로 들어가 API키를 생성한다.  
openAPI 계정이 없다면 만들어야 한다.  
https://platform.openai.com/account/api-keys

Create new secret key 버튼을 눌러 키를 생성하고 복사한다.  
![image1](/images/2023-03-17-vscode_with_gpt_1.png)  

setting에 들어가 Aicodehelper: Gptkey 항목에 복사한 키를 붙여넣는다.  
Aicodehelper: Language 항목에서 원하는 언어도 설정 할 수 있다.  

## 사용법

### 코드생성
원하는 것을 입력하고 ```Ctrl + Alt + Shift + G```를 누르면  
chatGPT에서 코드를 생성해서 넣어준다.  

아래는 간단한 로그인 화면을 생성해 달라는 요청을 처리한 결과이다.  
![image2](/images/2023-03-17-vscode_with_gpt_2.png)  
![image3](/images/2023-03-17-vscode_with_gpt_3.png)  
![image4](/images/2023-03-17-vscode_with_gpt_4.png)  
![image5](/images/2023-03-17-vscode_with_gpt_5.png)  

### 질문하기
질문도 바로 할 수 있는데 질문 내용을 입력한 후  
```Ctrl + Alt + Shift + M```을 누르면  
chatGPT의 답변을 가져와서 보여준다.  

![image6](/images/2023-03-17-vscode_with_gpt_6.png)  
![image7](/images/2023-03-17-vscode_with_gpt_7.png)  
![image8](/images/2023-03-17-vscode_with_gpt_8.png)  

### 그외 기능
- 코드 리뷰     ```Ctrl + Alt + Shift + C```  
- 코드 리펙토링 ```Ctrl + Alt + Shift + R```  
- 코드 주석달기 ```Ctrl + Alt + Shift + Z```  

## 주의사항
사용량에 따라 요금이 발생한다.  
이 익스텐션은 GPT-3.5-turbo 모델을 사용하는데  
사용한 토큰 수에 따라 요금이 발생하니 잘 확인해야 한다.  

사용량은 아래 페이지에서 확인 할 수 있다.  
<https://platform.openai.com/account/usage>

### 참고 

[ChatGPT를 VSCode안으로 데리고 오자 | 인공지능 코딩]  
<https://www.youtube.com/watch?v=SQPLPPb_LuE>