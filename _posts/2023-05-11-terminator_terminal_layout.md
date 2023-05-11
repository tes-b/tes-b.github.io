---
published: true
title:  "[Linux, ubuntu] 터미널 창 분할 (Terminator)"
categories: [Linux, ubuntu]
tag: [Linux, ubuntu, terminator]
---

## 터미네이터(terminaotr)

터미네이터(ternimator)라는 터미널인데 동시에 여러창을 켜서 작업을 해야 할 때 유용하다.  

![image0](/images/2023-05-11-terminator_terminal_layout_0.png)

하둡 구축시에 여러대의 컴퓨터에서 반복 명령을 해야 할 때가 많아 작업 효율이 상당히 높아졌다.  
위의 이미지 처럼 하나의 창에 여러개의 터미널을 분할 해서 띄울 수 있고  
동시에 여러 터미널에 같은 동작을 실행 할 수 있다.  

## 설치
```
sudo apt install terminator
```

## 사용법

### 창분할

터미널에서 우클릭  
가로 분할은 ```Split Horizontally```  
세로 분할은 ```Split Vertically```  

분할 창 닫기는 해당 창에서 ```우클릭``` >> ```Close```

### 동시 입력

![image0](/images/2023-05-11-terminator_terminal_layout_1.png)  
왼쪽 상단 아이콘 클릭 후 ```Broadcast all``` 선택  
해체하려면 ```Broadcast off``` 선택

### 레이아웃 저장

현재 레이아웃을 저장하고 싶으면   
우클릭 >> ```Preferences``` >> ```Layout``` 에 들어간 뒤
```add```를 누르면 현재의 레이아웃이 추가되고 ```save```를 눌러 저장하면 된다. 
![image0](/images/2023-05-11-terminator_terminal_layout_2.png) 


