---
published: True
title:  "[ubuntu] 우분투 20.04 텐서플로우 GPU 설치하기 -2"
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


## 설치할 버전 확인

<https://www.tensorflow.org/install/source?hl=ko#linux>  

이 사이트에서 설치하려는 텐서플로 버전에 맞는 CUDA와 cuDNN 버전을 확인 할 수 있다.

![image1](/images/2022-11-13-tensorflow_ubuntu_2_0.png) 

tensorflow-gpu-2.7.0 버전을 설치할 예정이니  
CUDA 11.2와 cuDNN 8.1 을 설치하면 된다.  


## nvidia 드라이버 설치

```ubuntu-drivers devices```

위의 커멘드를 입력하면 설치가능한 드라이버 목록이 나온다.  

![image1](/images/2022-10-22-Ubuntu-boot-fail_4.png)  

드라이버는 아래 명령으로 원하는 버전을  설치 할 수 있다.

```
sudo apt install <드라이버>
```

 나는 470 버전을 설치했다.  
```
sudo apt install nvidia-driver-470
```

설치후 재부팅을 해줘야 한다.  

재부팅후 ```nvidia-smi``` 을 터미널에 입력하면 

![image1](/images/2022-11-13-tensorflow_ubuntu_2_7.png) 

이런식으로 정보가 나온다.  
오른쪽 상단의 CUDA Version이 지원가능한 최신 버전이다.  


## CUDA toolkit 설치


<https://developer.nvidia.com/cuda-toolkit-archive>  

위 링크에 들어가 CUDA Toolkit 11.2를 찾는다.  

환경에 맞게 선택하고 Installer Type은 runfile을 선택한다.  

![image1](/images/2022-11-13-tensorflow_ubuntu_2_1.png)  

아래쪽 Base Installer 소스를 복사해서 터미널에 입력한다.  
설치화면이 뜨는데 시간이 좀 걸린다.  

아래 화면이 나오면 continue를 선택

![image1](/images/2022-11-13-tensorflow_ubuntu_2_2.png)  

그다음 accept를 입력하고 엔터.

![image1](/images/2022-11-13-tensorflow_ubuntu_2_3.png)  

Driver 항목에서 스페이스바를 누르면 체크가 해제된다.  
체크해제 후 Install 항목을 선택하면 설치가 된다.  

![image1](/images/2022-11-13-tensorflow_ubuntu_2_4.png)  

설치가 끝나면 다음 명령으로 CUDA Toolkit 관련 설정을   
환경 변수에 추가하고 적용한다.  
```
$ sudo sh -c "echo 'export PATH=$PATH:/usr/local/cuda-11.2/bin' >> /etc/profile"
$ sudo sh -c "echo 'export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-11.2/lib64' >> /etc/profile"
$ sudo sh -c "echo 'export CUDADIR=/usr/local/cuda-11.2' >> /etc/profile"
$ source /etc/profile
```

설치가 잘 되었는지 확인하려면 터미널에 ```nvcc -V```를 입력하면 된다.  

## cuDNN 설치

<https://developer.nvidia.com/cudnn>

위의 링크에 접속한다.

Download cuDNN을 누르면 로그인을 하라고 한다.  
계정이 없다면 가입을 하고 있으면 로그인한다.  

로그인하면 아래처럼 나오는데 라이센스 동의에 체크하고  
Archived cuDNN Releases를 클릭.  

![image1](/images/2022-11-13-tensorflow_ubuntu_2_5.png)  

그리고 위에서 확인한 대로 8.1 버전을 다운 받는다.  
Download cuDNN v8.1.0 (January 26th, 2021), for CUDA 11.0,11.1 and 11.2 를 누르고  
cuDNN Library for Linux (x86_64)를 누르면 다운이 시작된다.  

![image1](/images/2022-11-13-tensorflow_ubuntu_2_6.png)  

다운이 끝났으면 압축을 풀고 파일을 복사해준다.  
```
$ sudo cp cuda/include/cudnn* /usr/local/cuda/include
$ sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64
$ sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
```

링크를 다시 걸어줘야 한다...   
라고 블로그에 써있어서 하긴 했는데 왜하는지는 잘 모르겠다.
```
$ sudo ln -sf /usr/local/cuda-11.2/targets/x86_64-linux/lib/libcudnn_adv_train.so.8.1.0 /usr/local/cuda-11.2/targets/x86_64-linux/lib/libcudnn_adv_train.so.8
$ sudo ln -sf /usr/local/cuda-11.2/targets/x86_64-linux/lib/libcudnn_ops_infer.so.8.1.0  /usr/local/cuda-11.2/targets/x86_64-linux/lib/libcudnn_ops_infer.so.8
$ sudo ln -sf /usr/local/cuda-11.2/targets/x86_64-linux/lib/libcudnn_cnn_train.so.8.1.0  /usr/local/cuda-11.2/targets/x86_64-linux/lib/libcudnn_cnn_train.so.8
$ sudo ln -sf /usr/local/cuda-11.2/targets/x86_64-linux/lib/libcudnn_adv_infer.so.8.1.0  /usr/local/cuda-11.2/targets/x86_64-linux/lib/libcudnn_adv_infer.so.8
$ sudo ln -sf /usr/local/cuda-11.2/targets/x86_64-linux/lib/libcudnn_ops_train.so.8.1.0  /usr/local/cuda-11.2/targets/x86_64-linux/lib/libcudnn_ops_train.so.8
$ sudo ln -sf /usr/local/cuda-11.2/targets/x86_64-linux/lib/libcudnn_cnn_infer.so.8.1.0 /usr/local/cuda-11.2/targets/x86_64-linux/lib/libcudnn_cnn_infer.so.8
$ sudo ln -sf /usr/local/cuda-11.2/targets/x86_64-linux/lib/libcudnn.so.8.1.0  /usr/local/cuda-11.2/targets/x86_64-linux/lib/libcudnn.so.8
```



그다음 아래 명령어를 사용해 추가된 라이브러리를 시스템에서 찾을 수 있도록 한다.

```
sudo ldconfig
```

ldconfig가 뭔지 잘 모르겠지만 공유 라이브러리 캐시를 다시 설정하는 명령어라고 한다.  

설정이 잘 되었는지 확인하려면 루트디렉토리에서 다음명령을 입력한다.  
```
ldconfig -N -v $(sed 's/:/ /' <<< $LD_LIBRARY_PATH) 2>/dev/null | grep libcudnn
```
```
출력

libcudnn_adv_train.so.8 -> libcudnn_adv_train.so.8.1.0
libcudnn_adv_infer.so.8 -> libcudnn_adv_infer.so.8.1.0
libcudnn_ops_infer.so.8 -> libcudnn_ops_infer.so.8.1.0
libcudnn.so.8 -> libcudnn.so.8.1.0
libcudnn_ops_train.so.8 -> libcudnn_ops_train.so.8.1.0
libcudnn_cnn_train.so.8 -> libcudnn_cnn_train.so.8.1.0
libcudnn_cnn_infer.so.8 -> libcudnn_cnn_infer.so.8.1.0
```

## tensorflow 설치

드디어 텐서플로를 설치할 차례이다.  
```
pip install tensorflow
```
나의 경우 위의 명령으로 gpu 버전이 아닌 텐서플로를 설치하니 gpu인식이 되지 않았다.  
```
pip install tensorflow-gpu=2.7
```
그래서 위의 명령어로 gpu 2.7 버전을 설치하니 정상적으로 gpu를 인식했다.  

처음에는 최신버전인 2.10버전을 설치했었는데 코드 실행과정에서 에러들을 좀 마주쳤다.  
역시 최신버전은 깔면 안된다는 교훈을 또다시 얻고 2.7 버전을 설치하니 안정적으로 돌아갔다.

### 설치 환경

![image5](/images/2022-10-22-Ubuntu-boot-fail_5.png)  