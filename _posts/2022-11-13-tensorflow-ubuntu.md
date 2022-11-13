---
published: true
title:  "[ubuntu] 우분투 20.04 텐서플로 GPU 설치하기"
categories: ubuntu
tag: [ubuntu, tensorflow, cuda, cuDNN]
---

## 드라이버 초기화

드라이버나 쿠다의 버전 충돌이 일어날 수 있어  
설치전 기존의 nvidia 드라이버와 cuda를 삭제한뒤  
깔끔한 환경에서 설치하는 것을 추천!  

엔비디아와 쿠다를 삭제하는 명령어  

> sudo apt-get remove --purge '^nvidia-.*'   
sudo apt-get autoremove --purge 'cuda*'

## cuda toolkit 설치

아래 명령어로 쿠다 툴킷을 설치한다.
> sudo apt install nvidia-cuda-toolkit

설치가 끝나면 아래 명령어로 버전을 확인한다.
> nvcc -V 

![image5](/images/2022-11-13-tensorflow_ubuntu_0.png) 

10.1버전이 설치 된 것을 볼 수 있다.  

그다음 아래 명령어로 설치된 위치를 확인하고 복사해둔다.  
보통은 
/usr/lib/cuda  
/usr/include/cuda.h  
경로가 될것이다. 

> whereis cuda

![image5](/images/2022-11-13-tensorflow_ubuntu_1.png) 

## cuDNN 설치

아래 다운로드 페이지에서 cuDNN를 다운받는다.

![https://developer.nvidia.com/rdp/cudnn-archive](https://developer.nvidia.com/rdp/cudnn-archive)

7.6.5  for cuda10.1 을 눌러 cuDNN Library for Linux를 받으면 된다.

![image5](/images/2022-11-13-tensorflow_ubuntu_2.png)  

<https://www.tensorflow.org/install/source?hl=ko#linux>  
페이지에서 자세한 추가적인 버전 지원 사항을 볼 수 있다.

다운로드가 끝나면 압축을 풀어준다.  
명령어를 사용해도 되고 더블클릭해서 Extract를 해도 된다.
> tar -xvzf cudnn-10.1-linux-x64-v7.6.5.32.tgz

그다음 위에서 복사했던 경로로 파일을 복사해준다.
> sudo cp cuda/include/cudnn.h /usr/lib/cuda/include/  
sudo cp cuda/lib64/libcudnn* /usr/lib/cuda/lib64/  

![image5](/images/2022-11-13-tensorflow_ubuntu_3.png)  

cuDNN에 대한 권한을 설정해준다.
>  sudo chmod a+r /usr/lib/cuda/include/cudnn.h  
sudo chmod a+r /usr/lib/cuda/lib64/libcudnn*

![image5](/images/2022-11-13-tensorflow_ubuntu_4.png)  

## 환경변수 설정

.bashrc 파일을 열어 아래의 환경변수를 추가해준다.  
```export LD_LIBRARY_PATH=/usr/lib/cuda/lib64:$LD_LIBRARY_PATH```  
```export LD_LIBRARY_PATH=/usr/lib/cuda/include:$LD_LIBRARY_PATH```

vim에서 수정하거나 gedit 등의 텍스트 에디터로 수정해도 된다.
> sudo vim ~/.bashrc  

혹은 아래 커멘드로 바로 넣어줘도 된다.

```echo 'export LD_LIBRARY_PATH=/usr/lib/cuda/lib64:$LD_LIBRARY_PATH' >> ~/.bashrc```   
```echo 'export LD_LIBRARY_PATH=/usr/lib/cuda/include:$LD_LIBRARY_PATH' >> ~/.bashrc```

![image5](/images/2022-11-13-tensorflow_ubuntu_6.png)  


그다음 아래 명령어로 환경변수를 적용한다.
> source ~/.bashrc 

<span style="color:blue">**만약 터미널로 zsh를 사용하고 있다면**</span>  
.bashrc가 아닌 .zshrc파일에 위의 환경변수를 넣어줘야 한다.  
환경변수 적용 역시 .zshrc파일로 해야한다.

## 텐서플로 GPU 설치

![image5](/images/2022-11-13-tensorflow_ubuntu_5.png)  

cuda 10.1이 호환되는 tensorflow-2.1.0 버전은  
파이썬 3.7 버전과 호환되기 때문에 먼저 그게 맞게 가상환경을 만든다.  

> conda create -n tf_gpu python=3.7

가상환경을 활성화 시킨 뒤  
> conda activate tf_gpu

tensorflow-2.1.0 버전을 설치한다.  
> conda install -c anaconda tensorflow-gpu==2.1.0

## 설치 확인

> conda list  

명령어를 사용해 tensorflow-gpu가 설치된 것을 확인한다.  
![image5](/images/2022-11-13-tensorflow_ubuntu_7.png)  


파이썬을 켜고 아래 코드를 입력해 gpu를 인식하는지 확인 할 수 있다.  

```py
import tensorflow as tf
print(tf.config.list_physical_devices("GPU")) #1
print(tf.test.is_gpu_available()) #2
```
1번 print가 []로 나온다면 gpu 인식이 되지않는다는 뜻이고  
2번 print 역시 False로 나올 것이다.

정상적으로 동작한다면 아래와 같은 print를 볼 수 있다.
```py
#1 print
[PhysicalDevice(name='/physical_device:GPU:0', device_type='GPU')]

#2 print
True
```

### 실행 환경

우분투 20.04 LTS 환경에서 설치했습니다.

![image5](/images/2022-10-22-Ubuntu-boot-fail_5.png)  

### 참고한 페이지
<https://seonghyuk.tistory.com/58>  
<https://lsjsj92.tistory.com/627>
