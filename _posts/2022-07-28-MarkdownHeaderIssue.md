---
published: true
title:  "마크다운 헤더 미적용 문제"
categories: Diary
toc: true
toc_sticky: true
author_profile: true
sidebar:
    nav: "docs"
search: true
tag: [github, markdown, header]
---

이 포스트는 7월 28일 하루 동안의 삽질에 관한 기록이다.

# 문제의 시작.

이미지에 주석을 달고 싶어서 헤더5(#####) 를 사용해 이미지 위에 주석을 썼다.  
하지만 실제 포스팅을 하고 보니...   헤더가 적용이 안된건지 이미지 위의 글자가 너무 컸다.  

![caption_0](/images/HeaderIssue_0.PNG)*원했던 그림.* 

![caption_1](/images/HeaderIssue_1.PNG)*실제 포스팅.*  



<br>
그래서 테스트를 해봤다.
<br><br>

![caption_2](/images/HeaderIssue_2.PNG)*헤더 테스트와 VSCode미리보기*
![caption_3](/images/HeaderIssue_3.PNG)*헤더4번부터 적용이 안되는 모습.*
<br> <br>

글씨 크기가 변하지 않는 걸로 미루어 헤더 3번까지는 되고 4번부터 적용이 안되는 느낌이었다. 
<br><br>

# 구글링...구글링...

그래서 구글링을 해봤지만 어디서도 답을 찾을 수 없었다.  
스텍오버플로에 비슷하게 헤더가 표시되지 않는 문제를 겪는 사람들은 있었지만 나와 완전히 같은 이슈가 있다는 글은 없었다.   

<br>
그렇게 구글을 헤엄친지 몇시간...  
<br>  

안되는 헤더 문제는 무시하기로 했다. 😅  

왜냐면 이미지에 주석을 다는 방법을 찾았기 때문!  


<br>

# 마크다운으로 이미지에 주석 넣기... 근데?!
  
  마크다운으로 이미지에 주석을 다는 방법은 간단했다.  
  
```markdown
  ![](path_to_image)
  *image_caption*
```

이렇게 마크다운을 작성하면  
<br>

```html
<p>
    <img src="path_to_image" alt>
    <em>image_caption</em>
</p>
```
이렇게 html 코드가 생성되는 것을 이용해  
<br>

```css
img + em {
  display: block;
  text-align: center;
  font-size: .8rem;
  color: light-grey;
}
```
이렇게 코드를 css 파일에 넣어주면 되는 것!  
<br>
이 작업을 하려 _base.scss 파일에 들어간 나는  
발견하고 말았다...😲  

 ![caption_5](/images/HeaderIssue_5.PNG)*폰트 크기 정의*
<br>  

헤더의 폰트 크기가 정의된 부분이 여기 있었다.

헤더 4번부터 적용이 안된 것이 아니라 그냥 글자 크기가 거의 차이가 없는 것이었다.  
<br>

폰트 크기를 차이가 나게 조정하고  
![caption_6](/images/HeaderIssue_6.PNG)  

<br>
다시 테스트를 해보니  

![caption_7](/images/HeaderIssue_7.PNG)*차이가 느껴진다!!*
<br><br>

# 결론

하루종일 붙잡고 있던 헤더 문제는 이렇게 허무하게 끝났다.  
솔직히 좀 부끄러워서 포스팅을 할까 말까 고민했는데  
코딩을 하다보면 일주일 동안 전전긍긍하던 문제가 사소한 오타 때문인 경우라던가,  
재현이 안되는 버그가 사실 사용자의 독특한 습관 때문이었거나 하는 경우가 있다.  
그런게 또 코딩의 재미 아니겠나?!...  
라고 변명해본다.
