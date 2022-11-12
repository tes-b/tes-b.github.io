---
published: true
title:  "[ubuntu] 우분투 키 입려 지연시간 설정"
categories: ubuntu
tag: [ubuntu]
---

우분투에서 키를 꾹 누를 때의 입력지연시간 설정하기  

## xset 이용

```
xset r rate 300 30
```
rate 뒤의 첫번째 숫자는 키를 누르고 있을 때  
첫글자 입력 후 두번째 글자가 입력 될때까지 걸리는 시간이며  
단위는 ms 이다.

rate 뒤의 두번째 숫자는 얼마나 빠르게 입력을 반복 할 것인가 이다.  
1초에 몇번을 입력하는 것인지 정하면 된다.  
30 이면 1초에 30번을 입력한다.

### 출처

https://www.shrimp.pe.kr/10201