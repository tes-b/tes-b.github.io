---
published: true 
title:  "[AWS, Jupyter] EC2에 주피터 노트북 실행 및 pyspark로 HDFS에서 파일 불러오기"
categories: [hadoop, spark, AWS, Jupyter]
tag: [hadoop, spark, AWS, EC2, Jupyter]
---

EC2에 주피터를 설치하고 실행한 뒤 pyspark를 사용해 하둡에 올라간 파일을 불러온다.  


EC2에 하둡 스파크 클러스터 구축하기  

1편: <https://tes-b.github.io/hadoop/spark/aws/hadoop_spark_on_ec2_1/>  
2편: <https://tes-b.github.io/hadoop/spark/aws/hadoop_spark_on_ec2_2/>  
3편: <https://tes-b.github.io/hadoop/spark/aws/hadoop_spark_on_ec2_3/>  

## 주피터 설치

```
sudo apt-get update
```
pip가 없는 경우 설치
```
sudo apt-get install python3-pip
```
주피터 노트북 설치
```
sudo apt install notebook
```

## 설정 및 실행

파이썬 실행
```
python3
```

```passwd```를 실생하면 패스워드가 생성된다.  

```py
>>> from notebook.auth import passwd
>>> passwd()
```
```"argon2:$argon2id.....``` 이런 식으로 생성된다.  
설정시 필요하니 어디에 복사해둔다.  
```exit()```로 빠져나온다.  

환경성정 파일 생성  
```
jupyter notebook --generate-config
```
환경설정 파일 수정
```
sudo vim /home/ubuntu/.jupyter/jupyter_notebooke_config.py
```
아래 내용을 입력한다.  
```
c=get_config()
c.NotebookApp.password = u'<복사해둔 패스워드>'
c.NotebookApp.ip = '0.0.0.0' 
c.NotebookApp.notebook_dir = '/home/ubuntu' # 원하는 경로
```

주피터 노트북 실행
```
jupyter-notebook
```

### 에러

설정 파일 생성시 아래와 같은 에러가 생겼었다.   
```
jupyter notebook --generate-config
Traceback (most recent call last):
  File "/usr/local/bin/jupyter-notebook", line 5, in <module>
    from notebook.notebookapp import main
  File "/home/ubuntu/.local/lib/python3.8/site-packages/notebook/notebookapp.py", line 43, in <module>
    from jinja2 import Environment, FileSystemLoader
  File "/usr/lib/python3/dist-packages/jinja2/__init__.py", line 33, in <module>
    from jinja2.environment import Environment, Template
  File "/usr/lib/python3/dist-packages/jinja2/environment.py", line 15, in <module>
    from jinja2 import nodes
  File "/usr/lib/python3/dist-packages/jinja2/nodes.py", line 23, in <module>
    from jinja2.utils import Markup
  File "/usr/lib/python3/dist-packages/jinja2/utils.py", line 656, in <module>
    from markupsafe import Markup, escape, soft_unicode
ImportError: cannot import name 'soft_unicode' from 'markupsafe' (/home/ubuntu/.local/lib/python3.8/site-packages/markupsafe/__init__.py)
```

```markupsafe``` 다른 버전 설치하니 정상정으로 실행이 되었다.  
```
pip install markupsafe==2.0.1
```

## 포트 설정  
주피터 노트북은 기본적으로 ```8888``` 포트로 실행된다.  
그래서 인바운드 설정에서 ```8888``` 포트를 열어줘야 한다.  

![Image0](/images/2023-05-21-hadoop_spark_on_ec2_jupyter_0.png)  

```http://<인스턴스 public IP>:8888```로 접속할 수 있다.  

## 백그라운드 실행 
 
터미널을 끈 상태에서도 주피터 실행을 유지하려면  
```ctl+z```를 눌러 일시정지 한 뒤 아래 명령어를 실행한다.  
```
# 일시정지된 프로세스 백그라운드로 돌림
bg

# 소유권 포기
disown -h 
```

## 하둡 파일 시스템에 파일 업로드  

로컬에서 서버로 파일 전송
```
scp test.parquet nn1:~/
```
하둡 시스템으로 업로드
```
hdfs dfs -put test.parquet /data/
```

## 파이스파크 초기화

```py
from pyspark.sql import SparkSession, SQLContext

spark = SparkSession.builder \
        .appName('Read parquet from hdfs') \
        .getOrCreate()
```
```
spark
```

![Image0](/images/2023-05-21-hadoop_spark_on_ec2_jupyter_1.png)  


## 하둡에서 파일 불러오기

```
df = spark.read.parquet('hdfs:///data/test.parquet')
```
```
df.show()
```

![Image0](/images/2023-05-21-hadoop_spark_on_ec2_jupyter_2.png)  


### 참고 

<https://deepmal.tistory.com/25>