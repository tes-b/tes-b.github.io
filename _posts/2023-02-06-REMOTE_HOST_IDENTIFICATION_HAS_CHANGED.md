---
published: True
title:  "SSH 접속 오류(WARNING : REMOTE HOST IDENTIFICATION HAS CHANGED)"
categories: Etc
tag: [ssh, ubuntu, ec2]
---

ssh로 aws ec2 새로운 인스턴스에 접속하려 하니 아래와 같은 에러가 나왔다.  
```
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that the RSA host key has just been changed.
The fingerprint for the RSA key sent by the remote host is
어쩌구저쩌구..
```

## 해결방법

구글링 하니 ```ssh-keygen -R <서버 IP>```를 사용해서  
known_hosts의 내용을 갱신하는 방법이 있다고 하는데 나는 이방법으로 해결되지 않았다.  

그래서 /root/.ssh/known_hosts 경로로 들어가 파일의 내용을 전부 삭제했다.  

```
sudo nano /root/.ssh/known_hosts
```  

내용 전부 삭제한 뒤 다시 접속시도하니 정상적으로 접속이 된다.  


### 참고
<https://kingsong.tistory.com/127>