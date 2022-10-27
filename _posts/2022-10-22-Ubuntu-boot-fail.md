---
published: true
title:  "[ubuntu] 우분투 부팅 멈춤 현상 해결방법"
categories: ubuntu
tag: [ubuntu]
---

pci DPC: PR PIO log size 0 is invalid  
/dev/nvme0n1pX: clean, \*\*\*/\*\*\* files, blocks

## 우분투 부팅 실패

![image0](/images/2022-10-22-Ubuntu-boot-fail_0.png)  
우분투를 처음 설치하고 부팅을 하니 위의 화면에서  
멈추는 현상이 발생했다.  

처음부터 호락호락하지 않은 우분투...

## 해결 방법

에러 메세지로 구글링을 하니 해결한 분이 있어서 글을 참고했다.  
https://yskim0.github.io/troubleshooting/2021/01/26/Ubuntu-clean-files-blocks-trouble/

일단 리커버리 모드로 들어가야 한다.  

![image1](/images/2022-10-22-Ubuntu-boot-fail_1.png)*Advanced options for Ubuntu 선택*  

![image2](/images/2022-10-22-Ubuntu-boot-fail_2.png)*recovery mode 선택*

![image3](/images/2022-10-22-Ubuntu-boot-fail_3.png)*root로 들어간다.*

여기까지 오면 명령어를 입력 할 수 있다.  
아래 명령어를 입력한다.  
> sudo apt-get purge nvidia*

드라이버를 nvidia 그래픽 드라이버를 날려버리는 명령이다.  
드라이버는 부팅이 되면 그때 다시 찾아서 설치하면 된다.

위의 과정을 따라하니 일단 부팅까지는 성공!  

<details>
<summary>에러 처리</summary>
<div markdown="1">

나는 에러 없이 동작했지만 혹시 필요한 분들을 위해 남긴다.

참고한 블로그의 작성자분은 아래의 에러가 있었다고 한다.  
> dpkg was interrupted, you must manually run 'sudo dpkg --configure -a' to correct the problem

위의 에러가 발생 할 경우 아래의 명렁어를 입력하면 된다고 한다.  

> sudo dpkg --configure -a 

</div>
</details>


## 문제의 원인

결론부터 말하면 그래픽 드라이버 버전이 안맞았던 것 같다.

> ubuntu-drivers devices 

위의 커멘드를 입력하면 설치가능한 드라이버 목록이 나온다.  

![image1](/images/2022-10-22-Ubuntu-boot-fail_4.png)  

드라이버는 아래 명령으로 원하는 버전을  설치 할 수 있다.
> sudo apt install <드라이버>  

처음 그래픽 드라이버를 설치할 때 여기서  
recommeded라 써있는 515-open 버전 드라이버를 설치했는데  
> sudo apt install nvidia-driver-515-open

이게 내 랩탑과 맞지 않았던 것 같다. 

그래서 하위버전인 510을 설치하니 아예 메세지도 없이  
검은 화면만 뜨며 부팅이 안된다.(호락호락하지 않다...)

결국 더 낮은 버전인 470을 설치하니 정상적으로 부팅이 되었다.  
그래픽 드라이버도 정상적으로 동작했다.  
> sudo apt install nvidia-driver-470

### 사양 

![image5](/images/2022-10-22-Ubuntu-boot-fail_5.png)  