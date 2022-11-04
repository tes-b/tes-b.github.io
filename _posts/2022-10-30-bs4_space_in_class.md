---
published: false
title:  "[python] beautiful soup 공백있는 클래스 검색"
categories: python
tag: [python, bueatiful soup, bs4]
---

엄밀한 의미에서 HTML의 class명에 공백이 있는 것이 아니라 multiple classes라고 보는 것이 정확하지만,

 

편의상 '공백'이라고 설명하겠습니다 ^^

 

 

가장 간단한 방법은 CSS 셀렉터를 사용하는 것입니다.

 

공백을 '.'으로 대체하는 것으로 해결할 수 있습니다.

 

예컨대 소스가 <tr class="study for us">라면 soup.select_one('tr.study.for.us')라고 찾으면 됩니다 ^^

https://studyforus.com/tipnknowhow/789053