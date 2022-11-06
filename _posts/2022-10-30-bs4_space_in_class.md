---
published: true
title:  "[python] beautiful soup 공백있는 클래스 검색"
categories: python
tag: [python, bueatiful soup, bs4]
---


beautiful soup으로 html을 파싱할 때 다음과 같은 클래스가 있을 때가 있다.
```html
<tr class="name with space">
```

이 경우에 beautiful soup의 select 함수를 사용할 때  
```soup.select_one('.name with space')```라고 입력하게 되면 검색이 안된다.  
이때는 ```soup.select_one('tr.name.with.space')```로 입력하면 된다.

### 출처
https://studyforus.com/tipnknowhow/789053