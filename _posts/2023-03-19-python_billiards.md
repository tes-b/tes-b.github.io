---
published: True
title:  "[Programmers, Python3, Lv.2] 당구 연습"
categories: CodingTests
tag: [Programmers, Python3, Database]
---

## 문제

프로그래머스의 마스코트인 머쓱이는 최근 취미로 당구를 치기 시작했습니다.

머쓱이는 손 대신 날개를 사용해야 해서 당구를 잘 못 칩니다. 하지만 끈기가 강한 머쓱이는 열심히 노력해서 당구를 잘 치려고 당구 학원에 다니고 있습니다.

오늘도 당구 학원에 나온 머쓱이에게 당구 선생님이"원쿠션"(당구에서 공을 쳐서 벽에 맞히는 걸 쿠션이라고 부르고, 벽에 한 번 맞힌 후 공에 맞히면 원쿠션이라고 부릅니다) 연습을 하라면서 당구공의 위치가 담긴 리스트를 건네줬습니다. 리스트에는 머쓱이가 맞춰야 하는 공들의 위치가 담겨있습니다. 머쓱이는 리스트에 담긴 각 위치에 순서대로 공을 놓아가며 "원쿠션" 연습을 하면 됩니다. 이때, 머쓱이는 항상 같은 위치에 공을 놓고 쳐서 리스트에 담긴 위치에 놓인 공을 맞춥니다.

머쓱이와 달리 최근 취미로 알고리즘 문제를 풀기 시작한 당신은, 머쓱이가 친 공이 각각의 목표로한 공에 맞을 때까지 최소 얼마의 거리를 굴러가야 하는지가 궁금해졌습니다.

당구대의 가로 길이 m, 세로 길이 n과 머쓱이가 쳐야 하는 공이 놓인 위치 좌표를 나타내는 두 정수 startX, startY, 그리고 매 회마다 목표로 해야하는 공들의 위치 좌표를 나타내는 정수 쌍들이 들어있는 2차원 정수배열 balls가 주어집니다. "원쿠션" 연습을 위해 머쓱이가 공을 적어도 벽에 한 번은 맞춘 후 목표 공에 맞힌다고 할 때, 각 회마다 머쓱이가 친 공이 굴러간 거리의 최솟값의 제곱을 배열에 담아 return 하도록 solution 함수를 완성해 주세요.

단, 머쓱이가 친 공이 벽에 부딪힐 때 진행 방향은 항상 입사각과 반사각이 동일하며, 만약 꼭짓점에 부딪힐 경우 진입 방향의 반대방향으로 공이 진행됩니다. 공의 크기는 무시하며, 두 공의 좌표가 정확히 일치하는 경우에만 두 공이 서로 맞았다고 판단합니다. 공이 목표 공에 맞기 전에 멈추는 경우는 없으며, 목표 공에 맞으면 바로 멈춘다고 가정합니다.

## 해답

더 나은 풀이 방법이 있겠지만 나는 아래와 같이 풀었다.

```py
def solution(m, n, startX, startY, balls):
    answer = []
    for ball in balls:
        diffX = startX-ball[0] # X차이
        diffY = startY-ball[1] # Y차이
        
        left = (startX+ball[0])**2 + (diffY**2)          # 왼쪽 쿠션
        right = ((m-startX)+(m-ball[0]))**2 + (diffY**2) # 오른쪽 쿠션
        top = (diffX**2) + ((n-startY)+(n-ball[1]))**2   # 위쪽 쿠션
        bottom = (diffX**2) + (startY+ball[1])**2        # 아래쪽 쿠션
        
        if diffX == 0: # X축 같은 선상일 때
            if diffY > 0: # 아래쪽 방향 쿠션 안됨
                res = min(left, right, top)        
            else : # 위쪽 방향 쿠션 안됨
                res = min(left, right, bottom)    
                
        elif diffY == 0: # Y축 같은 선상일 때
            if diffX > 0: # 왼쪽 쿠션 안됨
                res = min(right, top, bottom)        
            else : # 오른쪽 쿠션 안됨
                res = min(left, top, bottom)      
                
        else: # 같은 축 없을 때
            res = min(left, right, top, bottom)
            
        # 결과 append
        answer.append(res)
    return answer
```

## 풀이

기본 아이디어는 모든 방향의 거리를 구한 뒤 최소값을 구하는 것이다.  


1. 시작지점과 타겟의 X,Y축의 차이를 구한다.
    ```py
        diffX = startX-ball[0] # X차이
        diffY = startY-ball[1] # Y차이   
    ``` 
2. 왼쪽,오른쪽,위,아래 방향 쿠션을 튕겼을 때 거리를 전부 구한다.  

    예시) 왼쪽 쿠션에 튕길 때의 최소 거리
    - ```((왼쪽 쿠션부터 시작지점 X축 까지 거리 + 왼쪽쿠션부터 목표지점 X축까지 거리)**2) + Y축에서 두지점 사이의 거리**2```  


3. 두 공이 같은 선상에 있을 경우 시작지점에서 쿠션방향 사이에 목표지점이 있으면 쿠션이 안되므로 예외처리.  
    ```py
    if diffX == 0: # X축 같은 선상일 때
        if diffY > 0: # 아래쪽 방향 쿠션 안됨
            res = min(left, right, top) # 아래는 빼고 최소값을 구한다.
        else : # 위쪽 방향 쿠션 안됨
            res = min(left, right, bottom) # 위는 빼고 최소값을 구한다.
            
    elif diffY == 0: # Y축 같은 선상일 때
        if diffX > 0: # 왼쪽 쿠션 안됨
            res = min(right, top, bottom) # 왼쪽은 빼고 최소값을 구한다.
        else : # 오른쪽 쿠션 안됨
            res = min(left, top, bottom) # 오른쪽은 빼고 최소값을 구한다.
            
    else: # 같은 축 없을 때
        res = min(left, right, top, bottom) # 모든 방향에서 최소값을 구한다.
    ```
    
