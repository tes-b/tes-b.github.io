---
published: false
title:  "[ubuntu] 우분투 충전 제한 설정"
categories: ubuntu
tag: [ubuntu]
---

우분투에서 배터리 충전 제한하는법
/sys/class/power_supply/BAT1
위의 경로로 들어간다 
BAT1이 아닌 BAT0 일 수도 있다.
그다음 이 파일을 확인하면 이게 베터리 충전을 어디까지 할 것인지 결정하는 파라미터이다. 100으로 설정되어있다.
charge_control_end_threshold

echo 60 | sudo tee /sys/class/power_supply/BAT1/charge_control_end_threshold

터미널에서 위의 명령어로 원하는 값으로 바꾼다. 
나는 60으로 바꿨다.

그런데 이 세팅은 재부팅을 할 경우 초기화가 된다.
그래서 재부팅시 값을 다시 설정하는 세팅을 해줘야 한다.

다시 터미널에서 
sudo gedit /etc/crontab 입력
crontab 파일이 열리면 
아래에 명령을 추가해 준다.

@reboot root echo 60 > /sys/class/power_supply/BAT1/charge_control_end_threshold

그리고 아래에 이렇게 메세지를 넣어준다.

출처
https://www.youtube.com/watch?v=BacV_hvaXfU