---
published: true
title:  "[ubuntu] 우분투 스왑(swap) 메모리 설정. 메모리 부족 해결"
categories: [ubuntu, Error]
tag: [ubuntu, Error]
---

## 메모리 부족

파일 분석을 위해 parquet 파일을 불러오는 도중 아래와 같은 애러가 나며 실패함.
```
Canceled future for execute_request message before replies were done.  
The Kernel crashed while executing code in the the current cell or a previous cell. 
Please review the code in the cell(s) to identify a possible cause of the failure. 
Click here for more info. View Jupyter log for further details.
```

커널이 크래시가 나는 현상이었는데 GPT에 물어보니 주로 메모리가 부족할때 나타나는 현상이라고 한다.  
해당 파일은 2.6gb 짜리 parquet 파일이었는데  
csv로 치면 25gb의 큰 파일이었기 때문에 메모리에 한번에 올라가기에 무리가 있었다.  

## swap

swap이라고 메모리가 부족할 때 하드디스크의 일부를 메모리로 사용하는 기능이 있는데  
당연히 메모리 보다 속도는 느리지만 메모리가 부족해서 위와 같이 터지는 현상을 막을 수 있다.  
윈도우의 가상메모리 같은 기능이다.  

확인해보니 세팅값은 2gb 였는데 이것을 8gb로 늘려주려 한다.  

![Image0](/images/2023-05-16-ubuntu_swap_setting_0.png)

### swap 파일 확인

```
sudo free -m
```
또는
```
sudo swapon -s
```
명령을 사용하면 아래와 같이 정보가 나오거나 없다면 나오지 않을 것이다. 

![Image1](/images/2023-05-16-ubuntu_swap_setting_1.png)

실행 중이라면 아래 명령으로 swap을 중지한다.  
```
sudo swapoff -a
```


### swap 생성

```
sudo fallocate -l 8G /swapfile
```
위 명령어로 ```swapfile```을 생성하면 된다.  
```-l``` 뒤에 원하는 용량을 넣어서 원하는 대로 세팅하면 된다.  

그다음 파일 권한을 설정하고 활성화한다.  
```
sudo chmod 600 /swapfile # 권한 수정
sudo mkswap /swapfile    # 활성화 준비
sudo swapon /swapfile    # 활성화
```

![Image2](/images/2023-05-16-ubuntu_swap_setting_2.png)


재부팅 후 계속 사용하려면 ```/etc/fstab``` 파일에 내용을 추가해야 한다.  

```
# 파일 편집
sudo vim /etc/fstab 

## 내용 추가
/swapfile swap swap defaults 0 0
```

## 동작 확인 

다시 파일을 불러오니 swap을 활용해 커널 크래시 없이 파일을 불러오는데 성공했다.  
![Image3](/images/2023-05-16-ubuntu_swap_setting_3.png)

### swap 파일 삭제

더이상 swap 사용을 원하지 않으면 스왑 파일을 지우면 된다.  
```
# 스왑 비활성화
sudo swapoff -v /swapfile

# 파일 편집
sudo vim /etc/fstab

# 아래 라인 삭제
/swapfile swap swap defaults 0 0

 # swap 파일 삭제
sudo rm /swapfile
```


## 참고
<https://lovedh.tistory.com/entry/Ubuntu-2004%EC%97%90%EC%84%9C-swap-%EB%A9%94%EB%AA%A8%EB%A6%AC-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0>