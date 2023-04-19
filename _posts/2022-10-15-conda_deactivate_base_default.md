---
published: true
title:  "아나콘다 터미널 실행시 base deactivate 하기"
categories: [anaconda]
tag: [anaconda, anaconda3]
---

터미널 실행시 자동으로 base 가상환경이 실행되는게 싫다면  
다음 명령어로 해제 할 수 있다.
```
conda config --set auto_activate_base false
```