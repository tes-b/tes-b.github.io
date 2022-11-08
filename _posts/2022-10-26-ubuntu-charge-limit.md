---
published: true
title:  "[ubuntu] 우분투 충전 제한 설정"
categories: ubuntu
tag: [ubuntu]
---

노트북의 배터리 수명 관리를 위해 배터리 충전량을 제한하고 싶을 때

## 우분투에서 배터리 충전 제한하는 방법  

/sys/class/power_supply/BAT1  
위의 경로로 들어간다.  
(BAT1이 아닌 BAT0 일 수도 있다.)  
![image0](/images/2022-10-26-ubuntu-charge-limit_0.png)

charge_control_end_threshold 을 열면 100이라는 숫자가 나온다.  
베터리 충전을 몇% 까지 할 것인지 결정하는 파라미터이다. 

```echo 60 | sudo tee /sys/class/power_supply/BAT1/charge_control_end_threshold```

터미널에서 위의 명령어를 사용해도 되고 텍스트 에디터로 값을 변경해 줘도 된다.  
나는 60으로 바꿨다.

그런데 이 세팅은 재부팅을 할 경우 초기화가 된다.
그래서 재부팅시 값을 다시 설정하는 세팅을 해줘야 한다.

다시 터미널에서 
```sudo gedit /etc/crontab``` 입력하고  
crontab 파일이 열리면 아래에 명령을 추가해 준다.

```
@reboot root echo 60 > /sys/class/power_supply/BAT1/charge_control_end_threshold
```
![image1](/images/2022-10-26-ubuntu-charge-limit_1.png)

### 출처
https://www.youtube.com/watch?v=BacV_hvaXfU