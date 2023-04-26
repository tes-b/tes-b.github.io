---
published: true
title:  "[Error, python] ModuleNotFoundError: No module named 'distutils.cmd'"
categories: [Error, python]
tag: [Error, python]
---

## 문제

librosa 라이브러리 설치도중 아래와 같은 에러 만남  
```
ModuleNotFoundError: No module named 'distutils.cmd'
```

## 해결방법
```
sudo apt-get install python3.8-distutils 
```
파이썬 버전은 원하는 것으로 하면 된다.  
