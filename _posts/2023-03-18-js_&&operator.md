---
published: true
title:  "[JS] 논리연산자(&&, ||) 단축평가"
categories: JS
tag: [JS]
---

자바스크립트에서 논리연산자 ||, && 는 왼쪽부터 오른쪽으로 평가를 진행하는데,  
중간에 평가 결과가 나오면 끝까지 가지않고 평과 결과를 반환한다.  
이를 단축평가라고 하면 피연산자의 타입을 반환하지 않고 그대로 반환한다.  

```js
"apple" || "banana";  // "apple"
"apple" && "banana"; // "banana"
```
## 논리합 || 연산자
왼쪽 그대로 반환
```js
true || false; // true
true || true; // true

"apple" || false; // "apple"
"apple" || true; // "apple"
```

오른쪽 그대로 반환
```js
false || true; // true
false || false; // false

false || "banana"; // "banana"

"apple" || "banana"; // "apple"
```

## 논리곱 && 연산자

왼쪽 피연산자가 false면 바로 false로 평가  
```js
false && true; // false
false && false; // false

false && "banana"; // false

null && false; // null
```

왼쪽이 true면 오른쪽 그대로 반환  
```js
true && true; // true
true && false; // false

"apple" && true; // true
"apple" && false; // false
```
### 참고

아래 블로그 글을 요약했습니다.   
<https://curryyou.tistory.com/193>