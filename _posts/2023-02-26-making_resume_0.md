---
published: False
title:  "[JS] 왕초보 워들(wordle) 만들기"
categories: JS
tag: [JS]
---


만약 개발 언어를 하나라도 알고있다면  
다른 언어를 배우는 빠른 방법은  
뭔가를 만들어 보는 것이라고 생각한다.  

그래서 이번에 자바스크립트도 배울겸    
간단한 게임을 만들어 보려고 한다.  

## Wordle  
![이미지]
워들이라는 게임을 만들어 볼 건데 5글자의 단어를 알아내는 게임이다.  
5글자의 단어를 입력하면 글자의 위치가 맞는지 단어에 포함된 글자인지 알려주고  
이를 바탕으로 정답을 유추하는 간단한 게임이다.  
숫자야구를 해봤다면 어떤 게임인지 이해가 빠를 것이다.  

## 시작

일단 나는 js와 css에 대해 거의 아는게 없다.  
그래서 인터넷에 올라온 튜토리얼을 따라하면서  
언어의 문법과 동작방식을 공부해보려 한다.  

## 밑반찬 세팅  
일단 기본 밑반찬을 세팅한다.  
```wordle.html``` 파일을 만들어주고  
vscode를 사용하면 ! 만 치면 
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    
</body>
</html>
```

기렇게 기본 상차림이 깔리는데 body 태그 안에 메인요리를 집어넣을 것이다.  

## 재료손질  
그다음 WORDLE이라고 쓰고  
글자를 입력할 input을 만들어준다.  
5글자니까 5개 만들어준다.  
```html
<body>
    <p>WORDLE</p>
    <style>
        input {
            width: 50px;
            height: 50px;
            font-size: 40px;
            text-align: center;
        }
    </style>
    <input class="input">
    <input class="input">
    <input class="input">
    <input class="input">
    <input class="input">
    <button>제출</button>
</body>
```