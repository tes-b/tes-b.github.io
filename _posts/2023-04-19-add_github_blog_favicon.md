---
published: true
title:  "깃허브 블로그에 favicon 넣기 (minimal mistakes, realfavicongenerator 사용안함)"
categories: [Etc, github]
tag: [github, minimal-mistakes, favicon]
---

minimal mistakes 테마를 기준으로 작성했는데  
다른테마도 비슷한 방식으로 사용 할 수 있을 것이다.  

## favicon 이미지 준비

favicon 으로 사용할 이미지를 준비한다.  
```16x16```이나 ```32x32``` 정도면 적당하다.  
나는 ```32x32``` ```png```파일을 사용했다.  

이미지를 ```/assets``` 경로에 넣는다.  
따로 폴더를 만들어도 된다.  
나는 ```/assets/img/favicon(32x32).png``` 이렇게 넣었다.  

## favicon 적용

```_includes/head/custom.html```   
위의 파일을 모든 페이지에서 포함하기 때문에  
여기에 적용하면 다른 페이지들도 일괄적으로 적용 된다.  
아래와 같이 추가한다.  

```html
<link rel="shortcut icon" href="{{ '/assets/img/favicon(32x32).png' | relative_url }}">
```