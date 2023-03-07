---
published: True
title:  "[JS] 애니메이션 끝날 때 기다리기 - 비동기 프로그래밍"
categories: JS
tag: [JS, Promise, async]
---

## 문제

자바 스크립트를 배우려고 wordle게임 클론 코딩을 했다.  
기본 뼈대의 클론코딩이 끝나고 게임을 다듬는 중 문제가 생겼는데  

애니메이션이 끝나기 전에 다음 프로세스가 실행되는 것이었다.  
모든 애니메이션이 끝나고 결과창이 나와야 하는데  
결과창이 먼저 나와 애니메이션을 가리는 것이었다.  

검색해 보니 Promise 등의 비동기 처리 방식을 사용해  
선행작업이 끝나면 다음 프로세스가 실행되게 해야 한다고 한다.  

사용방법은 chatGPT가 짜준 예제코드를 보고 카피했다.  

자바스크립트가 처음이라 자세한 설명은 못하지만  
아래와 같은 방법으로 처리하니 잘 동작했다.  

## 처리 방법

1. 프로미스들을 담을 배열을 만든다.  
2. 해야하는 작업을 새로운 ```Promise``` 객체를 만들어 배열에 push
3. 이때 ```element``` 에 ```animationend``` 이벤트를 추가해야 하는것 같다.  
애니메이션 엔드라는 이름만 봐도 알 것 같지만 한번 검색해보았다.  
> The animationend event is fired when a CSS Animation has completed.  
애니메이션이 끝나는걸 확인하는 이벤트인 듯 하다.  
4. ```Promise.all```을 사용해서 프로미스들을 처리하고 ```.then()```으로  
이후 작업들을 처리하면 된다.  

## 코드 스니펫

```js
const promises = [];

    currentWordArr.forEach(function (letter, index) {
        promises.push(new Promise((resolve) => {
            setTimeout(() => {
                const tileColor = getTileColor(letter, index);

                const letterId = firstLetterId + index;
                const letterEl = document.getElementById(letterId);
                letterEl.classList.add("animate__flipInX");
                letterEl.style = `background-color:${tileColor};border-color:${tileColor}`;
                letterEl.addEventListener("animationend", () => {
                    resolve(); // resolve the promise when the animation is completed
                });
                updateGuessedKeys(letter, tileColor);
            }, interval * index);
        }));
    });

    Promise.all(promises).then(() => {
        //   console.log("All animations are done!");
        // 모든 애니메이션이 끝나면 결과 확인한다.
        if (currentWord === word) { // 정답
            result = true;
            openResultBox();
        }
    });
```
