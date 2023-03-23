---
published: True
title:  "[Programmers, Python3, Lv.2] 리코쳇 로봇"
categories: CodingTests
tag: [Programmers, Python3, Database]
---

## 문제

리코쳇 로봇이라는 보드게임이 있습니다.

이 보드게임은 격자모양 게임판 위에서 말을 움직이는 게임으로, 시작 위치에서 목표 위치까지 최소 몇 번만에 도달할 수 있는지 말하는 게임입니다.

이 게임에서 말의 움직임은 상, 하, 좌, 우 4방향 중 하나를 선택해서 게임판 위의 장애물이나 맨 끝에 부딪힐 때까지 미끄러져 이동하는 것을 한 번의 이동으로 칩니다.

다음은 보드게임판을 나타낸 예시입니다.
```
...D..R
.D.G...
....D.D
D....D.
..D....
```
여기서 "."은 빈 공간을, "R"은 로봇의 처음 위치를, "D"는 장애물의 위치를, "G"는 목표지점을 나타냅니다.
위 예시에서는 "R" 위치에서 아래, 왼쪽, 위, 왼쪽, 아래, 오른쪽, 위 순서로 움직이면 7번 만에 "G" 위치에 멈춰 설 수 있으며, 이것이 최소 움직임 중 하나입니다.

게임판의 상태를 나타내는 문자열 배열 board가 주어졌을 때, 말이 목표위치에 도달하는데 최소 몇 번 이동해야 하는지 return 하는 solution함수를 완성하세요. 만약 목표위치에 도달할 수 없다면 -1을 return 해주세요.

## 코드

```py
import re
import numpy as np

def go(board,count):
    list_search = []
    lenX = len(board[0]) # 보드 가로 길이
    lenY = len(board)    # 보드 세로 길이
    # print(board, lenX, lenY)

    # # 시작, 도착점 확인
    G = R = None
    for i,line in enumerate(board):
        g = r = -1
        if "G" in line:
            g = line.index("G")

        if "R" in line:    
            r = line.index("R")

        if count in line:
            for j, c in enumerate(line):
                if c == count :
                    list_search.append(i*lenX + j)
        
        if g != -1:
            G = i*lenX + g
            board[G//lenX][G%lenX] = 0
            list_search.append(G)
        if r != -1:
            R = i*lenX + r
        
    # print("G : ",G)
    # print("R : ",R)
    # print(list_search)

    # # 진행할수있는 노드 없으면 실패
    if not list_search : return -1

    count += 1
    
    for sp in list_search:
        if board[sp//lenX][sp%lenX-1] == "D": # 왼쪽 막힘
            for i in range(sp+1,lenX*(sp//lenX)+lenX): # 오른쪽 돌면서 카운트 표시
                c = board[i//lenX][i%lenX]
                if c in [".","R"]:
                    board[i//lenX][i%lenX] = count
                elif c in ["G"] or type(c)==int: continue
                else: break

        if board[sp//lenX][sp%lenX+1] == "D": # 오른쪽 막힘
            for i in range(sp-1,lenX*(sp//lenX),-1): # 왼쪽 돌면서 카운트 표시
                c = board[i//lenX][i%lenX]
                if c in [".","R"]:
                    board[i//lenX][i%lenX] = count
                elif c in ["G"] or type(c)==int: continue
                else: break

        if board[sp//lenX-1][sp%lenX] == "D": # 위쪽 막힘
            for i in range(sp+lenX,lenX*lenY,lenX): # 아래쪽 돌면서 카운트 표시
                c = board[i//lenX][i%lenX]
                if c in [".","R"]:
                    board[i//lenX][i%lenX] = count
                elif c in ["G"] or type(c)==int: continue
                else: break

        if board[sp//lenX+1][sp%lenX] == "D": # 아래쪽 막힘
            for i in range(sp-lenX,0,-lenX): # 위쪽 돌면서 카운트 표시
                c = board[i//lenX][i%lenX]
                if c in [".","R"]:
                    board[i//lenX][i%lenX] = count
                elif c in ["G"] or type(c)==int: continue
                else: break
    
    # 출발점 확인
    if all("R" not in l for l in board):
        for b in board:
            print(b)
        return board[R//lenX][R%lenX]
    
    return go(board,count)

def solution(board):   
    answer = 0
    boardX = len(board[0])
    new_board = []

    # 보드를 D로 감싼다.
    # 판정 편하게 하기 위함
    
    new_board.append(["D"]*(boardX+2))
    for line in board:
        new_board.append(["D",*list(line),"D"])
    new_board.append(["D"]*(boardX+2))

    # 보드 표시
    for b in new_board:
        print(b)

    answer = go(new_board,0)

    return answer

```

## 풀이

나의 경우 골인지점인 G에서 시작해서 출발점을 찾아가는 방법으로 풀었다.  
효과적인 방법인지는 잘 모르겠다.  
코딩은 좀 복잡하게 했지만 진행 방식은 아래와 같다.

### 답이 있는 경우
아래와 같은 보드판이있을 때
```
...D..R
.D.G...
....D.D
D....D.
..D....
```

G에 도달할 수있는 모든 곳에 1표시를 한다.  
G에 도달하기 위해서는 옆에 벽이나 멈출 수 있는 장애물(D)이나 벽이 있어야 한다.  
보드에서 G위에 D가 있기때문에 G에 도달할 수 있는 모든 위치는 아래쪽이 된다.  
표시하면 아래와 같다.  
```
...D..R
.D.G...
...1D.D
D..1.D.
..D1...
```

그다음 1표시된 모든 지점에 도달 할 수 있는 곳에 2표시를 한다.  
셋째줄에 있는 1은 오른쪽에 D가 있기때문에 왼쪽줄에 2 표시를 한다.  
넷째줄에 있는 1은 주변에 벽이나 장애물이 없어서 도달 불가능이다.  
다섯째줄은 같은 방식으로 오른쪽줄에 2표시를 해준다.  
```
...D..R
.D.G...
2221D.D
D..1.D.
..D1222
```

다시 모든 2에 대해 반복한다.  
```
3..D..R
3D.G...
2221D.D
D3.13D3
.3D1222
```
이 과정을 반복하면 진행은 아래와 같다.  
```py
# 3
344D..R
3D.G...
2221D.D
D3413D3
43D1222

# 4
344D..R
3D5G...
2221D.D
D3413D3
43D1222

# 5
344D..R
3D5G666
2221D.D
D3413D3
43D1222

# 7
344D777
3D5G666
2221D.D
D3413D3
43D1222
```

R에 도달 할 때까지 반복하면 R위치에 쓰여지는 숫자가 최소 거리가 된다.  

### 답이 없는 경우
```
R..
DG.
...
```
위 보드처럼 같이 답이 없는 경우는 다음과 같이 진행된다.  

```py
#G
R..
DG1
...

#1
R..
DG1
...

# 2로 진행 불가
```
여기서 다음 진행할 위치가 없는 경우 골인이 불가한 보드다.  
