---
published: false
title:  "[docker] 도커 permission denied"
categories: docker
tag: [docker]
---



> Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/images/create?fromImage=docker%2Fwhalesay&tag=latest": dial unix /var/run/docker.sock: connect: permission denied

위와 같은 에러를 만났을 때

루트권한이 필요해서 그런듯?

유저한테 어플리케이션 사용 권한 주기

> sudo usermod -aG docker {사용자명}

> sudo usermod -aG docker $USER
- -a : 사용자를 추가하는 명령어(append)
- -G : 그룹 옵션

sudo : super user do


### 출처
https://shanepark.tistory.com/250