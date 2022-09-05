---
published: true
title:  "깃허브 블로그에 수식 쓰기 - miniaml-mistakes"
categories: Etc
tag: [github, math, minimal-mistakes]
---

Jekyll 깃허브 블로그에서 마크다운용 수식기호을 넣어도  
그냥은 멋드러진 수식으로 나오지 않는다.   
수식을 표시하려면 몇가지 세팅이 필요하다.  


## mathjax_support.html 파일 생성
_include 폴더에 mathjax_support.html 라는 파일을 생성하고 아래 내용을 넣어준다.
```html
<script type="text/x-mathjax-config">
MathJax.Hub.Config({
    TeX: {
      equationNumbers: {
        autoNumber: "AMS"
      }
    },
    tex2jax: {
    inlineMath: [ ['$', '$'] ],
    displayMath: [ ['$$', '$$'] ],
    processEscapes: true,
  }
});
MathJax.Hub.Register.MessageHook("Math Processing Error",function (message) {
	  alert("Math Processing Error: "+message[1]);
	});
MathJax.Hub.Register.MessageHook("TeX Jax - parse error",function (message) {
	  alert("Math Processing Error: "+message[1]);
	});
</script>
<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>
```
## _default.html 수정
_layout/default.html 파일을 열고 \<head> 태그 안에 다음 내용을 추가한다.
```py
    {% if page.use_math %} 
	{% include mathjax_support.html %}
    {% endif %}
```
## _config.yml 파일 수정
_config.yml 파일을 열고 #Conversion 을 검색해 해당 부분을 수정해준다.

```yml
# Conversion
markdown: kramdown
highlighter: rouge
lsi: false
excerpt_separator: "\n\n"
incremental: false
```
그다음 가장  defaults: 태그 아래의 values: 태그 안에 해당 내용을 추가한다.
```yml
defaults:
    value:
        use_math: true
```
이렇게 하면 모든 페이지에 디폴트로 적용되지만  
페이지별로 따로 설정하려면 포스트 작성시  
페이지 상단에 따로 ```use_math: true``` 를 넣어도 된다.


## 수식 테스트

```
$10 * 20 = 200$\
$
  \int_0^\infty \frac{x^3}{e^x-1}\,dx = \frac{\pi^4}{15}
$
```
$10 * 20 = 200$\
$
  \int_0^\infty \frac{x^3}{e^x-1}\,dx = \frac{\pi^4}{15}
$