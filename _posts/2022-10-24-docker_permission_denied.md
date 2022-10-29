---
published: true
title:  "[docker] 도커 permission denied"
categories: docker
tag: [docker]
---


> Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/images/create?fromImage=docker%2Fwhalesay&tag=latest": dial unix /var/run/docker.sock: connect: permission denied

위와 같은 에러를 만났다.

루트 권한 없이 그냥 Docker명령어를 사용하면  
루트권한이 필요하다는 것이다.

명령어 앞에 sudo를 써서 루트 권한으로 실행하거나  
**sudo: super user do**

유저한테 어플리케이션 사용 권한을 주면 된다.  

> sudo usermod -aG docker {사용자명}

> sudo usermod -aG docker $USER
- -a : 사용자를 추가하는 명령어(append)
- -G : 그룹 옵션

도커 뿐 아니라 다른 어플리케이션에도 가능하다.



### 출처
https://shanepark.tistory.com/250