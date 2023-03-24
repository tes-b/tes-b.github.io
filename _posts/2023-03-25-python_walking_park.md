---
published: True
title:  "[Programmers, Python3, Lv.1] 공원 산책"
categories: CodingTests
tag: [Programmers, Python3, Database]
---

## 문제

지나다니는 길을 'O', 장애물을 'X'로 나타낸 직사각형 격자 모양의 공원에서 로봇 강아지가 산책을 하려합니다. 산책은 로봇 강아지에 미리 입력된 명령에 따라 진행하며, 명령은 다음과 같은 형식으로 주어집니다.

["방향 거리", "방향 거리" … ]
예를 들어 "E 5"는 로봇 강아지가 현재 위치에서 동쪽으로 5칸 이동했다는 의미입니다. 로봇 강아지는 명령을 수행하기 전에 다음 두 가지를 먼저 확인합니다.

주어진 방향으로 이동할 때 공원을 벗어나는지 확인합니다.
주어진 방향으로 이동 중 장애물을 만나는지 확인합니다.
위 두 가지중 어느 하나라도 해당된다면, 로봇 강아지는 해당 명령을 무시하고 다음 명령을 수행합니다.
공원의 가로 길이가 W, 세로 길이가 H라고 할 때, 공원의 좌측 상단의 좌표는 (0, 0), 우측 하단의 좌표는 (H - 1, W - 1) 입니다.

공원을 나타내는 문자열 배열 park, 로봇 강아지가 수행할 명령이 담긴 문자열 배열 routes가 매개변수로 주어질 때, 로봇 강아지가 모든 명령을 수행 후 놓인 위치를 [세로 방향 좌표, 가로 방향 좌표] 순으로 배열에 담아 return 하도록 solution 함수를 완성해주세요.

## 코드

```py
def solution(park, routes):
    W = len(park[0])
    H = len(park)
    sX = sY = 0
    
    
    for y, line in enumerate(park):
        x = line.find("S")
        if x != -1:
            sX = x
            sY = y
            break

    for route in routes:
        r = route.split(" ")
        dir = r[0]
        step = int(r[1])
        
        if dir == "E":
            if sX+step >= W : continue
            if "X" in park[sY][sX+1:sX+step+1]: continue
            sX += step
        elif dir == "W" :
            if sX-step < 0: continue
            if "X" in park[sY][sX-step:sX]: continue
            sX -= step
        elif dir == "S":
            if sY+step >= H: continue
            if "X" in [line[sX] for line in park][sY+1:sY+step+1] : continue
            sY += step
        elif dir == "N":
            if sY-step < 0: continue
            if "X" in [line[sX] for line in park][sY-step:sY]: continue
            sY -= step

    answer = [sY, sX]
    return answer
```

## 풀이

1. S 위치를 찾는다. 
    ```py
    sX = sY = 0
    for y, line in enumerate(park):
        x = line.find("S")
        if x != -1:
            sX = x
            sY = y
            break
    ```

2. 공원의 가로 세로를 구한다.
    ```py
    W = len(park[0])
    H = len(park)
    ```
3. ```routes```를 ```for``` 문으로 돌린다.  
     ```split(" ")```으로 나눠서 방향과 걸음수를 뽑는다.  
    ```py
    for route in routes:
        r = route.split(" ")
        dir = r[0]
        step = int(r[1])
    ```
4. 방향에 따라 분기한다.  
    ```py
    if dir == "E":
        sX += step
    elif dir == "W" :
        sX -= step
    elif dir == "S":
        sY += step
    elif dir == "N":
        sY -= step
    ```

5. 공원을 벗어나는 경우 continue  
    - 현재좌표 + 걸음수 >= 공원크기  
    - 현재좌표 - 걸음수 < 0  
    ```py
    if dir == "E":
        if sX+step >= W : continue
        sX += step
    elif dir == "W" :
        if sX-step < 0: continue
        sX -= step
    elif dir == "S":
        if sY+step >= H: continue
        sY += step
    elif dir == "N":
        if sY-step < 0: continue
        sY -= step
    ```

5. 진행방향에 장애물이 있는 경우 continue   
    - 동쪽 : ```x좌표+1``` 에서 ```x좌표+걸음수+1``` 사이에 ```X```가 있는경우
    - 서쪽 : ```x좌표-걸음수``` 에서 ```x좌표``` 사이에 ```X```가 있는경우
    - 남쪽 : ```y좌표+1``` 에서 ```y좌표+걸음수+1``` 사이에 ```X```가 있는경우
    - 북쪽 : ```y좌표-걸음수``` 에서 ```y좌표``` 사이에 ```X```가 있는경우
    
    ```py
    if dir == "E":
        if sX+step >= W : continue
        if "X" in park[sY][sX+1:sX+step+1]: continue
        sX += step
    elif dir == "W" :
        if sX-step < 0: continue
        if "X" in park[sY][sX-step:sX]: continue
        sX -= step
    elif dir == "S":
        if sY+step >= H: continue
        if "X" in [line[sX] for line in park][sY+1:sY+step+1] : continue
        sY += step
    elif dir == "N":
        if sY-step < 0: continue
        if "X" in [line[sX] for line in park][sY-step:sY]: continue
        sY -= step
    ```
