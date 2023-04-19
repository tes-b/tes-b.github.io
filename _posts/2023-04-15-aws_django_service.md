---
published: True
title:  "[Django, AWS] Django 어플리케이션 AWS EC2로 배포하기 (gunicorn, nginx)"
categories: [Django, AWS]
tag: [Django, AWS, gunicorn, nginx, ubuntu]
---

이번에 django로 홈페이지를 제작하고 EC2를 사용해 배포했는데  
다음 배포시 참고하기 위해 기록용으로 정리해둔다.  

AWS EC2에서 인스턴스 생성 후 django 어플리케이션을 배포하는 방법을 정리했다.   
미들웨어는 gunicorn, 웹서버는 nginx를 사용한다.  



## 환경

서버는 AWS EC2 인스턴스르 사용하고  
운영체제는 우분투 20.04 버전을 사용했다.    

사용자 이름은 ```ubuntu``` 다.

EC2 인스턴스를 만들고 접속하는 방법은 아래 글을 보면 된다.  
  
**[AWS] AWS로 Flask 앱 배포하기**  <br>
<https://tes-b.github.io/aws/AWS_Flask/>


## 프로젝트 다운로드

aws ec2의 ubuntu 20.04의 경우 깃이 설치되어 있다.  
```
git clone <레포지토리주소>
```

## 설치

apt 업데이트
```
sudo apt update
```

pip 설치
```
sudo apt install python3-pip
```

## 가상환경 생성

프로젝트 디렉토리에서 가상환경 생성 
```
sudo apt install python3.8-venv
python3 -m venv venv
```

가상환경 실행
```
source venv/bin/activate
```

만약 ```DEBUG``` 옵션이 ```False```라면  
실행시 기존의 ```static``` 파일을 읽지 못하는데  
```
python manage.py collectstatic
```
위 명령으로 스테틱 파일을 모아줘야 한다.   
(아래에 좀더 자세히 써두었다.)

## 라이브러리 설치

requirements.txt 있으면
```
pip install -r requirements.txt
```

없으면 필요한 라이브러리들 설치 
```
pip install django
pip install djangorestframework
pip install djangorestframework-simplejwt
pip install django-cors-headers
pip install Django gunicorn
pip install conf
pip install wheel
pip install mysqlclient
```


## 설정파일 전송  
  
깃허브 등에 올릴 수 없는 장고 키나 기타 파일이 있으면 scp를 사용해 서버로 보낸다.  
```
scp -i [pem파일경로] [업로드할 파일 이름] [ec2-user계정명]@[ec2 instance의 public DNS]:~/[경로]
```
**[AWS] scp로 EC2 인스턴스에 파일 전송**  
<https://tes-b.github.io/aws/scp_send_file/>


## 서버 실행 테스트
```
python manage.py runserver 0.0.0.0:8000
```

```서버IP:8000``` 으로 접속을 확인해보자

EC2 보안그룹에서 8000 포트를 열지 않았으면 접속이 안된다.  

보안그룹에서 인바운드 규칙에 8000번 포트를 설정 해준다.  
![image0](/images/2023-04-15-aws_django_service_0.png)  
![image1](/images/2023-04-15-aws_django_service_1.png)  

이후 nginx를 사용하게되면 필요없어지니 접속 확인 후에는 다시 8000포트를 닫아도 상관이 없다.  


## gunicorn 실행 테스트

구니콘으로 실행 테스틀 해본다.  
```
gunicorn --bind 0.0.0.0:8000 <앱이름>.wsgi:application
```
  
구동이 되면 가상환경에서 빠져나온다.
```
deactivate
```

## gunicorn 서비스 등록  

서비스 등록 스크립트 생성  

```/etc/systemd/system/gunicorn.service``` 파일을 아래와 같은 내용으로 생성한다.  
파일 이름은 편의상 ```gunicorn```으로 했지만 다른 이름이어도 상관없다.  

```
[Unit]
Description=gunicorn daemon
After=network.target

[Service]
User=ubuntu
Group=www-data
WorkingDirectory=/home/ubuntu/<프로젝트이름>
ExecStart=/home/ubuntu/<프로젝트이름>/venv/bin/gunicorn \
        --workers 2 \
        --bind unix:/tmp/gunicorn.sock \
        config.wsgi:application

[Install]
WantedBy=multi-user.target
```

## gunicorn 서비스 실행

실행
```
sudo systemctl start gunicorn.service
```

상태확인
```
sudo systemctl status gunicorn.service
```   
상태를 확인했을 때 아래처럼 ```activate```가 나온다면 정상적으로 실행이 되고있다는 뜻이다.  
![image2](/images/2023-04-15-aws_django_service_2.png)

```서버IP:8000``` 으로 접속을 확인해보자


아래 명령어로 서버 시작될 때 실행되도록 세팅한다.  
```
sudo systemctl enable gunicorn.service
```  

정지하거나 재시작 하려면 아래 명령어를 사용하면 된다.  

정지
```
sudo systemctl stop gunicorn.service
```
재시작
```
sudo systemctl restart gunicorn.service
```

## nginx 설치

nginx 설치
```
sudo apt install nginx
```

## nginx 설정  


```/etc/nginx/sites-available/<프로젝트이름>```경로에 파일을 만들고 아래 내용을 넣는다.   

```
server {
        listen 80;
        server_name <서버의 고정 IP>;

        location = /favicon.ico { access_log off; log_not_found off; }

        location /static {
                root /home/ubuntu/<프로젝트이름>;
        }

        location / {
                include proxy_params;
                proxy_pass http://unix:/tmp/gunicorn.sock;
        }
}
```

**아래는 점프 투 장고에서 가져온 설명이다.**

listen 80 은 웹 서버를 80 포트로 서비스 한다는 의미이다. HTTP 프로토콜의 기본포트는 80이다.   
따라서 이제 http://3.37.58.70:8000/ 대신 포트를 생략하여 http://3.37.58.70 처럼 웹 브라우저에서 접속 할 수 있다.

```server_name``` 에는 여러분의 고정IP를 등록한다.

```location /static``` 은 정적 파일에 대한 설정으로 ```/static```으로 시작되는 URL 요청은  
Nginx가 ```/home/ubuntu/<프로젝트폴더>/static``` 디렉터리의 파일을 읽어서 처리한다는 설정이다.  

```location```은 ```location /static``` 에서 설정한 것 이외의 모든 요청은 Gunicorn이 처리하도록 하는 설정이다.  ```proxy_pass```는 동적 요청이 발생하면 해당 요청을 Gunicorn의 유닉스 소켓으로 보내라는 설정이다.

이와 같은 설정을 통해 ```/static``` 으로 시작되는 URL은 Nginx가 처리하고  
나머지 URL에 대해서는 Gunicorn이 처리하게 된다.

## nginx 설정

이제 작성한 mysite 파일을 Nginx가 환경 파일로 읽을 수 있도록 설정해야 한다.

위에서 작성한 파일을 링크한다.
```
sudo ln -s /etc/nginx/sites-available/<프로젝트이름> /etc/nginx/sites-enabled
```

nginx를 시작 한다. 
```
sudo systemctl start nginx
```

```서버 IP```로 접속해 확인해본다.  
접속이 되고 ```static```파일들이 문제없이 표시된다면 성공이다!

문제가 있을경우 nginx의 로그를 확인 할수 있다.  
```
/var/log/nginx/error.log
```

## 에러들  

### mysqlclient 설치 에러

에러내용
```
Collecting mysqlclient
  Using cached mysqlclient-2.1.1.tar.gz (88 kB)
    ERROR: Command errored out with exit status 1:
    command: /home/ubuntu/yogurt_town/venv/bin/python3 -c 'import sys, setuptools, tokenize; sys.argv[0] = '"'"'/tmp/pip-install-95lmmucl/mysqlclient/setup.py'"'"'; __file__='"'"'/tmp/pip-install-95lmmucl/mysqlclient/setup.py'"'"';f=getattr(tokenize, '"'"'open'"'"', open)(__file__);code=f.read().replace('"'"'\r\n'"'"', '"'"'\n'"'"');f.close();exec(compile(code, __file__, '"'"'exec'"'"'))' egg_info --egg-base /tmp/pip-install-95lmmucl/mysqlclient/pip-egg-info
        cwd: /tmp/pip-install-95lmmucl/mysqlclient/
    Complete output (15 lines):
    /bin/sh: 1: mysql_config: not found
    /bin/sh: 1: mariadb_config: not found
    /bin/sh: 1: mysql_config: not found
    Traceback (most recent call last):
      File "<string>", line 1, in <module>
      File "/tmp/pip-install-95lmmucl/mysqlclient/setup.py", line 15, in <module>
        metadata, options = get_config()
      File "/tmp/pip-install-95lmmucl/mysqlclient/setup_posix.py", line 70, in get_config
        libs = mysql_config("libs")
      File "/tmp/pip-install-95lmmucl/mysqlclient/setup_posix.py", line 31, in mysql_config
        raise OSError("{} not found".format(_mysql_config_path))
    OSError: mysql_config not found
    mysql_config --version
    mariadb_config --version
    mysql_config --libs
    ----------------------------------------
ERROR: Command errored out with exit status 1: python setup.py egg_info Check the logs for full command output.
```

아래 명령어 실행한 뒤 다시 설치
```
sudo apt-get install python3-dev default-libmysqlclient-dev build-essential
```


### 점프 투 장고 아무생각 없이 따라하다 생긴 에러  

처음에는 점프 투 장고만 보고 명령어를 그대로 복사해서 실행했다.  
```
gunicorn --bind 0.0.0.0:8000 conf.wsgi:application
```
그런데 다음과 같은 에러가 나고 실행이 안돼서 왜 안되는지 한참을 찾아다녔다.  
```
ModuleNotFoundError: No module named 'conf.wsgi'
```

알고보니 ```conf``` 부분을 자신의 ```settings.py```파일이 있는 앱이름으로 해야 했던 것.  
점프투 장고를 처음부터 따라했다면 앱이름이 ```conf```여서 문제없이 실행됐겠지만  
내경우는 페이지는 따로 만들고 배포과정만 따라하다가 이런 문제가 생겼었다.  

### static 파일 404에러  

개발시에는 ```DEBUG``` 세팅이 ```True```인 상태로 개발하지만     
배포시에는 ```False```로 바꾸게 되는데 이대로 실행시 ```static``` 파일 경로를 읽지 못하고 404에러가 난다.  

이때는 ```static```파일을 모아줘야 한다.  


먼저 ```settings.py``` 파일에서 ```STATIC_ROOT``` 경로를 정해준다.  
예시는 ```.static_root``` 경로로 정했지만 달라지거나 숨김폴더가 아니어도 상관없다.  

```py
# settings.py

import os

# Build paths inside the project like this: os.path.join(BASE_DIR, ...)
BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
ROOT_DIR = os.path.dirname(BASE_DIR)

# static files
STATIC_URL = '/static/'
STATICFILES_DIRS = (os.path.join('static/'),)
STATIC_ROOT = os.path.join(BASE_DIR, '.static_root')
```

그 다음 아래 명령어를 사용한다.  
```
python manage.py collectstatic
```

이 작업은 ```static``` 파일의 변동이 있으면 다시 해줘야 한다.  



## 참고  
점프 투 장고 <https://wikidocs.net/book/4223>  
Django 자습 <https://wikidocs.net/book/837>  
[Django] Ubuntu 18.04 에서 mysqlclient 설치오류 해결방법  
<https://iamiet.tistory.com/54>  
[Deploy] Django 프로젝트 배포하기 - 4. Static 파일  
<https://nachwon.github.io/django-deploy-4-static/>  