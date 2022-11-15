---
published: False
title:  "[ubuntu] 우분투 20.04 텐서플로우 GPU 설치하기"
categories: ubuntu
tag: [ubuntu, tensorflow, cuda, cuDNN]
---


<https://webnautes.tistory.com/1428>  
쿠다 설치 과정은 위의 블로그 내용을 대부분 가져온 것임을 밝힙니다.  
설치방법을 기록해두기 위해 작성하는 글입니다.

## 기존 환경 삭제

먼저 기존에 설치되어 있던 nvidia와 cuda를 지운다.

```
sudo apt-get purge nvidia*
sudo apt-get autoremove --purge 'cuda\*'

$ sudo apt-get autoremove
$ sudo apt-get autoclean
$ sudo rm -rf /usr/local/cuda*
```
.bashrc 나 /etc/profile에 추가되어 있는 CUDA관련 설정도 지워주자.  

## nvidia 드라이버 설치

```ubuntu-drivers devices```

위의 커멘드를 입력하면 설치가능한 드라이버 목록이 나온다.  

![image1](/images/2022-10-22-Ubuntu-boot-fail_4.png)  

드라이버는 아래 명령으로 원하는 버전을  설치 할 수 있다.

```sudo apt install <드라이버>```

 나는 470 버전을 설치했다.  
```sudo apt install nvidia-driver-470```

설치후 재부팅을 해줘야 한다.  

## CUDA 설치

재부팅후 ```nvidia-smi``` 을 터미널에 입력하면 

![image1](/images/2022-10-22-Ubuntu-boot-fail_4.png) 

이런식으로 정보가 나온다.  
오른쪽 상단의 CUDA Version이 지원가능한 최신 버전이다.  

<https://www.tensorflow.org/install/source?hl=ko#linux>  

이 사이트에서 설치하려는 텐서플로 버전에 맞는 CUDA와 cuDNN 버전을 확인 할 수 있다.

tensorflow-gpu-2.7.0 버전을 설치할 예정이니  
CUDA 11.2와 cuDNN 8.1 을 설치하면 된다.  

<https://developer.nvidia.com/cuda-toolkit-archive>

![image1](/images/2022-10-22-Ubuntu-boot-fail_4.png) 

환경에 맞게 선택하고 Installer Type은 runfile을 선택한다.  

아래의 Base Installer 소스를 복사해서 터미널에 입력한다.  
설치화면이 뜨는데 시간이 좀 걸린다.  
아래 화면이 나오면 continue를 선택

![image1](/images/2022-10-22-Ubuntu-boot-fail_4.png)  

그다음 accept를 입력하고 엔터.

![image1](/images/2022-10-22-Ubuntu-boot-fail_4.png)  

Driver 항목에서 스페이스바를 누르면 체크가 해제된다.  
체크해제 후 Install 항목을 선택하면 설치가 된다.  

----------------------------------------


위 블로그의 내용을 보고 cuda, cuDNN 까지 설치를 완료.
하지만 블로그처럼 gpu 버전이 아닌 텐서플로를 설치하니 gpu인식이 되지 않았다.  
tensorflow-gpu 를 설치하니 정상적으로 gpu를 인식했다.
처음에는 최신버전인 2.10버전을 설치했지만 코드 실행과정에서 에러들을 좀 마주쳤다. 역시 최신버전은 깔면 안된다는 교훈을 또다시 얻고 2.7 버전을 설치하니 안정적으로 돌아간다.