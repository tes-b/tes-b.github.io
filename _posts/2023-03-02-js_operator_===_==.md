---
published: true
title:  "[JavaScript] 연산자(===, ==) 차이"
categories: [JavaScript]
tag: [JavaScript]
---

## === 연산자

처음 자바스크립트를 사용했을 때 신기했던 연산자가 ```===```이다.  
C++이나 파이썬에서는 없는 연산자였기 때문이다.  

자바스크립트는 특이하게 같음을 확인하는 연산자가   
```==```와 ```===``` 두 개가 있다.  

```===```은 다른 언어의 ```==```과 비슷하다. 타입을 포함해 양쪽이 같은지를 확인한다.  
반면 ```==```는 양쪽 인자의 타입이 다르면 형변환을 해서 값을 비교한다.  

결과적으로 자바스크립트의 ```==```는 ```===```보다 느슨하다.  

## 예시

설명보다는 예시를 보면 이해가 빠르다.  
```js
var a = 42; 
var b = "42"; 
console.log(a == b); // true 
console.log(a === b); // false 

console.log(null == undefined); // true 
console.log(null === undefined); // false 

console.log(true == 1); // true 
console.log(true === 1); // false 
console.log(false == 0); // true 
console.log(false === 0); // false 

console.log(0 == "0"); // true 
console.log(0 === "0"); // false 
console.log(0 == ""); // true 
console.log(0 === ""); // false  
```

NaN(Not A Number)은 표시는 동일하지만 다르다.  
```js
console.log(NaN == NaN); // false 
console.log(NaN === NaN); // false 
```

배열의 경우 주소를 비교하기 때문에 내용이 같더라도 ```false```가 나온다.  
```js
var a = [1,2,3]; 
var b = [1,2,3]; 
console.log(a == b); // false 
console.log(a === b); // false 
```

아래 경우처럼 다른 변수에 복사하는 경우는 주소가 같기 때문에 ```true```가 나온다. 
```js
var a = [1,2,3]; 
var b = [1,2,3]; 
var c = b; 
console.log(b === c); // true 
console.log(b == c); // ture

var x = {}; 
var y = {}; 
var z = y; 
console.log(x == y) // false 
console.log(x === y) // false 
console.log(y === z) // true 
console.log(y == z) // true 
```

### 리스트 비교

잠시 옆길로 새면 예시와 같이 리스트를 ```==```연산자로 비교할 수 없기 때문에  
그래서 배열을 비교 할 때는 다른 방법을 사용해야 한다.  

정렬이 되어 있다면 ```JSON.stringify()```나 ```.to_String()```로 문자열로 변환해 비교하거나  
반복문을 이용해 비교해야 한다.  

(파이썬은 정렬되어 있다면 ```==```로 비교가 가능하다.)

### !=== 연산자  

```!=```연산자 처럼 ```!==``` 연산자도 있는데  
```===``` 연산자와 동일한 기준으로 다름을 판단한다.  

```js
console.log(42 != "42"); // false
console.log(42 !== "42"); // true
```