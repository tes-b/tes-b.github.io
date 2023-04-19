---
published: True
title:  "[miniconda] CondaFileIOError: '/home/ubuntu/bin/miniconda3/pkgs/envs/*/env.txt'. [Errno 2] No such file or directory: '/home/ubuntu/bin/miniconda3/pkgs/envs/*/env.txt'"
categories: [Anaconda, Error]
tag: [Anaconda, miniconda, ubuntu, Error]
---

## 미니콘다 설치 에러  
AWS EC2로 ubuntu 20.04 LTS 인스턴스 생성 후 미니콘다 설시 시도.  

```
sh Miniconda3-py39_22.11.1-1-Linux-x86_64.sh 
```

아래와 같은 오류로 설치가 안된다.    

> CondaFileIOError: '/home/ubuntu/bin/miniconda3/pkgs/envs/*/env.txt'. [Errno 2] No such file or directory: '/home/ubuntu/bin/miniconda3/pkgs/envs/*/env.txt'

## 해결방법

bash 명령어로 설치하면 정상적으로 설치가 된다.

```
bash Miniconda3-py310_22.11.1-1-Linux-x86_64.sh
```


### 참고
<https://github.com/ContinuumIO/anaconda-issues/issues/13110>

