---
published: False
title:  "[hadoop, AWS] EC2에 하둡 분산 환경 구축하기"
categories: [hadoop, AWS]
tag: [hadoop, AWS, EC2]
---

## 문제정의  

데이터 분석을 위해 사용할 데이터가 20gb가 넘어 in-memory 에서 사용하기 다소 힘들었음.  

## 계획  
하둡을 사용해 분산처리 시스템을 만들고 spark로 분석을 진행해본다.  
ec2 4대를 사용해 데이터를 처리할 예정

## 참고
설치 방법은 아래 블로그를 참고했고  
설치 버전과 몇몇 디테일만 내가 설치하려는 버전에 맞춰서 진행했다.  
<https://velog.io/@suran/Hadoop-%EC%99%84%EC%A0%84-%EB%B6%84%EC%82%B0-%EB%AA%A8%EB%93%9C-%EA%B5%AC%EC%B6%95-AWS-EC2>  

<https://inkkim.github.io/hadoop/AWS-EC2%EC%97%90%EC%84%9C-Hadoop-Cluster-%EA%B5%AC%EC%B6%95%ED%95%98%EA%B8%B0/>

개념에 대한 것은 아래 재생목록의 비디오로 공부했다.  
<https://www.youtube.com/watch?v=g6xIMSYjh0w&list=PLY-_9hx4ldZwYOjtfRT0MV2k9JcnTUYW2>  

데이터노드 추가

https://bloodguy.tistory.com/entry/Hadoop-%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%85%B8%EB%93%9C-%EC%B6%94%EA%B0%80%EC%82%AD%EC%A0%9C

https://parksuseong.blogspot.com/2019/11/adding-and-rebalancing-hadoop-datanodes.html?m=0

## 보안 그룹 설정

인바운드 포트 설정
[모든 노드에서 설정]

하둡 노드들끼리 통신을 하기 위해서는 인바운드 포트를 열어줘야 하기 때문에 다음과 같이 포트를 열어주시면 됩니다.

이름	Port  
Datanode Web UI	9864  
Datanode port	9866  
Datanode ipc port	9867  
Namenode port	9000  
Namenode Web UI	9870  
SecondaryNN port	9868  


## 내용

```
$ sudo apt install openjdk-11-jdk

$ sudo nano ~/.bashrc

=====
# bashrc 파일 안에 아래 내용을 작성
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
export PATH=$PATH:$JAVA_HOME/bin
=====

$ source ~/.bashrc

# 자바가 잘 설치되었는지 자바 버전 확인
$ java --version
```

하둡 3.3.5
```
$ wget https://dlcdn.apache.org/hadoop/common/hadoop-3.3.5/hadoop-3.3.5.tar.gz
```

압축풀기(설치)
```
$ tar xvzf hadoop-3.3.5.tar.gz
```

이름 변경
```
$ mv hadoop-3.3.5 hadoop
```

설치파일 삭제
```
$ rm hadoop-3.3.5.tar.gz
```

.bashrc 파일에 아래 내용 추가
```
# hadoop
export HADOOP_HOME=/home/ubuntu/hadoop
export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin:$HADOOP_HOME/etc/hadoop

# hadoop user
export HDFS_NAMENODE_USER=ubuntu
export HDFS_DATANODE_USER=ubuntu
export HDFS_SECONDARYNAMENODE_USER=ubuntu
export YARN_RESOURCEMANAGER_USER=ubuntu
export YARN_NODEMANAGER_USER=ubuntu
```

~ 는 홈 디렉토리
.bashrc  
자바를 설치하는 디렉토리  
하둡을 설치하는 디렉토리  


PATH  
실행파일 어딘지 알려주고  
어디서든 실행 할 수 있게 설정하는 것  
: 은 구분자
$ 표시는 일반 사용자를 표시  
# 은 root 사용자(super유저)를 의미

실행
```
$ source ~/.bashrc
```

## 호스트 이름 설정

[모든 노드에서 설정]

EC2 인스턴스를 처음 만들면 hostname이 ip-nn-nnn 이런식으로 기본 설정이 되어 있을 텐데, 그러면 다른 노드를 지정할 때 귀찮기 때문에 (뒤에 나오는 설정 파일 쓸 때도 힘듭니다..) hostname을 바꿔주도록 하겠습니다.  
  
이름	hostname  
네임노드	namenode01  
세컨더리 네임노드	namenode02  
데이터노드1	datanode01  
데이터노드2	datanode02  
  

```
sudo nano /etc/hostname
```

기존의 내용을 지우고 인스턴스에 맞는 이름을 적는다.
```
namenode01
```

재부팅한다.  
```
sudo reboot
```

## ip

hosts 설정
노드 간 통신을 할 때 해당 hostname, ip로 노드를 찾기 위함

제 ip 들을 공개하지 않기 위해, public ip, private ip 등으로 적겠습니다. 실제 적용하실 때는 aws 인스턴스 홈페이지에서 각자 본인의 public ip, private ip로 설정하시면 됩니다.

namenode01에서 설정
```
$ sudo vim /etc/hosts

private_ip namenode01  
public_ip namenode02  
public_ip datanode01  
public_ip datanode02  
```

### namenode 01  

Public IPv4 address  
 3.34.91.60 

Private IPv4 addresses  
 172.31.35.253


### namenode 02

Public IPv4 address  
 3.34.190.245

Private IPv4 addresses  
 172.31.43.52


### datanode 01

Public IPv4 address  
 54.180.152.92

Private IPv4 addresses  
 172.31.33.211


### datanode 02

Public IPv4 address  
 3.35.209.209 

Private IPv4 addresses  
 172.31.43.110




## 키전달
**LOCAL**
```
scp -i "hadoop_key.pem" /home/tesb/Documents/key/hadoop_key.pem ubuntu@ec2-3-34-91-60.ap-northeast-2.compute.amazonaws.com:~/.ssh
```

**namenode01**  
ssh 공개키 생성 후 다른 노드들에게 전달

ssh 키 생성
```
ubuntu@namenode01:~$ ssh-keygen -t rsa
```
이 명령어 입력하면 아래처럼 어디에 저장할지 비밀번호는 뭘로 할지 묻는다.  
나는 그냥 전부 엔터치고 넘어갔다.  
```
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ubuntu/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
```
그러면 아래처럼 키가 생성된다.  
```
Your identification has been saved in /home/ubuntu/.ssh/id_rsa
Your public key has been saved in /home/ubuntu/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:MtH6z4CQSFrV37gJmjxUCaNonORDO0xl3w0Ad1rJi8U ubuntu@namenode01
The key's randomart image is:
+---[RSA 3072]----+
| +.+==+=.        |
|B.+oo.OEo        |
|.X+  ++oo+       |
|.+o..o.+o .      |
|. .ooo+.So       |
|    =. =o        |
|     .. o        |
|         +       |
|          o      |
+----[SHA256]-----+
```

```
ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
```

authorized_keys 파일에 공개키 추가
```
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```

다른 노드들에게 공개키 전달
```
# namenode02 copy
$ scp -i hadoop_key.pem ~/.ssh/id_rsa.pub ubuntu@ec2-3-34-190-245.ap-northeast-2.compute.amazonaws.com:~/.ssh/

# datanode01 copy
$ scp -i hadoop_key.pem ~/.ssh/id_rsa.pub ubuntu@ec2-3-34-190-245.ap-northeast-2.compute.amazonaws.com:~/.ssh/

# datanode02 copy
$ scp -i hadoop_key.pem ~/.ssh/id_rsa.pub ubuntu@ec2-3-34-190-245.ap-northeast-2.compute.amazonaws.com:~/.ssh/
```

**namenode01, datanode01, datanode02**  
공개키를 authorized_keys에 전달
```
$ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```


네임노드에서 다른 노드로 접속 가능한지 확인  
```
$ ssh namenode02

$ ssh datanode01

$ ssh datanode02
```

## 방화벽 설정  

다른 블로그에서 봤을 때는 방화벽을 아예 다 내려주는 경우도 있던데, 저는 방화벽을 아예 내려주기에는 보안적인 문제가 있을 것이라 생각하여 서로의 노드 ip, 자기 자신의 port 정도만 열어주었습니다.  

ufw 는 방화벽으 설정하는 명령어이다.  
allow from은 말그대로 해당 아이피를 허용한다는 뜻이다.  
enable 은 방화멱을 활성화 한다는 뜻이다.

22번 포트는 ssh가 사용하는 포트이고  

98@@ 포트들은 아래와 같다.  

Datanode Web UI	9864  
Datanode port	9866  
Datanode ipc port	9867  
Namenode port	9000  
Namenode Web UI	9870  
SecondaryNN port	9868  


[namenode01에서 설정]
```
$ sudo ufw allow from [namenode02_public ip]
$ sudo ufw allow from [datanode01_public ip]
$ sudo ufw allow from [datanode02_public ip]

$ sudo ufw allow 22
$ sudo ufw allow 9000
$ sudo ufw allow 9870

$ sudo ufw enable
```
[namenode02에서 설정]
```
$ sudo ufw allow from [namenode01_public ip]
$ sudo ufw allow from [datanode01_public ip]
$ sudo ufw allow from [datanode02_public ip]

$ sudo ufw allow 22
$ sudo ufw allow 9868

$ sudo ufw enable
```
[datanode01에서 설정]
```
$ sudo ufw allow from [namenode01_public ip]
$ sudo ufw allow from [namenode02_public ip]
$ sudo ufw allow from [datanode02_public ip]

$ sudo ufw allow 22
$ sudo ufw allow 9864
$ sudo ufw allow 9866
$ sudo ufw allow 9867

$ sudo ufw enable
```
[datanode02에서 설정]
```
$ sudo ufw allow from [namenode01_public ip]
$ sudo ufw allow from [namenode02_public ip]
$ sudo ufw allow from [datanode01_public ip]

$ sudo ufw allow 22
$ sudo ufw allow 9864
$ sudo ufw allow 9866
$ sudo ufw allow 9867

$ sudo ufw enable
```

아래 명령으로 상태 확인 가능
```
$ sudo ufw status
```


## 이거는 잘 모르겠음..
[모든 노드에서 진행]
각 서버간 통신이 되어야 하둡도 진행이 가능하기 때문에 여기서 꼭 확인하고 넘어가주세요.

예. namenode01에서 실행 (다른 노드에서도 각자 telnet으로 통신 되는지 확인 필)  
```
$ telnet [namenode02_public ip] 9868  
$ telnet [datanode01_public ip] 9866  
$ telnet [datanode02_public ip] 9866  
```

## 설정 파일
파일 이름	내용  
core-site.xml	HDFS와 맵리듀스에서 공통적으로 사용할 환경설정 정보  
yarn-site.xml	맵리듀스 프레임워크에서 사용하는 셔플 서비스를 지정  
mapred-site.xml	맵리듀스에서 사용할 환경정보 설정  
hdfs-site.xml	HDFS에서 사용할 환경정보 설정  
workers (hadoop 3.x)	datanode 지정  



[모든 노드에서 진행]

core-site.xml

fs.defaultFS - 네임노드의 호스트명과 포트 번호 설정
```
$ vim $HADOOP_CONF_DIR/core-site.xml

=====
<configuration>
    <property>
            <name>fs.defaultFS</name>
            <value>hdfs://namenode01:9000</value>
    </property>
</configuration>
=====
```
yarn-site.xml

yarn.nodemanager.aux-services - 셔플 서비스 이름
yarn.resourcemanager.hostname - ResourceManager 노드에 설정
```
$ vim $HADOOP_CONF_DIR/yarn-site.xml

=====
<configuration>
   <property>
      <name>yarn.nodemanager.aux-services</name>
      <value>mapreduce_shuffle</value>
   </property>
    <property>
        <name>yarn.resourcemanager.hostname</name>
        <value>namenode01</value>
    </property>
</configuration>
=====
```
mapred-site.xml

mapreduce.framework.name
option - yarn모드라고 설정해줌
```
$ vim $HADOOP_CONF_DIR/mapred-site.xml
=====
<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
</configuration>
=====
```
[namenode01, namenode02에서 진행]

hdfs-site.xml

dfs.replication - 데이터 복제수, default 3
dfs.permissions - false
dfs.namenode.name.dir - fsimage가 저장될 경로, Namenode에 설정
dfs.namenode.http.address - namenode의 webui 포트지정
dfs.secondary.http.address - 세컨더리 네임노드 주소와 포트지정
```
$ vim $HADOOP_CONF_DIR/hdfs-site.xml

=====
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>2</value>
    </property>
    <property>
        <name>dfs.permissions</name>
        <value>false</value>
    </property>
    <property>
        <name>dfs.namenode.name.dir</name>
        <value>/home/ubuntu/hadoop/namenode_dir</value>
    </property>
    <property>
        <name>dfs.namenode.http.address</name>
        <value>namenode01:9870</value>
    </property>
    <property>
        <name>dfs.secondary.http.address</name>
        <value>namenode02:9868</value>
    </property>
</configuration>
=====
```
hadoop-env.sh

hadoop_conf 폴더나 자바 경로 등을 지정함
```
$ vim $HADOOP_CONF_DIR/hadoop-env.sh

# line 54
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64

# line 58
export HADOOP_HOME=/home/ubuntu/hadoop

# line 68
export HADOOP_CONF_DIR=${HADOOP_HOME}/etc/hadoop

# line 198
export HADOOP_PID_DIR=$HADOOP_HOME/pids
```
workers

datanode의 이름을 지정함
(slaves - hadoop 2.x , workers - hadoop 3.x)

```
# datanode용으로 쓸 노드의 호스트네임을 적어줌
$ vim $HADOOP_CONF_DIR/workers

=====
datanode01
datanode02
=====

[datanode01, datanode02에서 진행]

hdfs-site.xml
$ vim $HADOOP_CONF_DIR/hdfs-site.xml

=====
<configuration>
        <property>
                <name>dfs.replication</name>
                <value>2</value>
        </property>
        <property>
                <name>dfs.permissions</name>
                <value>false</value>
        </property>
        <property>
                <name>dfs.datanode.data.dir</name>
                <value>/home/ubuntu/hadoop/datanode_dir</value>
        </property>
</configuration>
=====
```
hadoop-env.sh
```
$ vim $HADOOP_CONF_DIR/hadoop-env.sh

# line 54
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64

# line 58
export HADOOP_HOME=/home/ubuntu/hadoop

# line 68
export HADOOP_CONF_DIR=${HADOOP_HOME}/etc/hadoop
```

## 포멧

[namenode01에서 진행]

포멧은 딱 한번만 하고 다시 하면 안된다.
```
$ hdfs namenode -format
```
예시 이미지 두장

## 데몬 실행
```
start-all.sh
```
위의 명령어는 파일 시스템과 얀(yarn)을 한번에 실행하는명령이다.
아래 두 명령을 한번에 해주는 명령어이다.
```
# 파일 시스템 실행
start-dfs.sh 
# 얀 실행 명령
start-yarn.sh
```


```
ubuntu@namenode01:~$ start-all.sh
WARNING: Attempting to start all Apache Hadoop daemons as ubuntu in 10 seconds.
WARNING: This is not a recommended production deployment configuration.
WARNING: Use CTRL-C to abort.
Starting namenodes on [namenode01]
namenode01: Warning: Permanently added 'namenode01,172.31.35.253' (ECDSA) to the list of known hosts.
Starting datanodes
datanode01: WARNING: /home/ubuntu/hadoop/logs does not exist. Creating.
datanode02: WARNING: /home/ubuntu/hadoop/logs does not exist. Creating.
Starting secondary namenodes [namenode02]
namenode02: WARNING: /home/ubuntu/hadoop/pids does not exist. Creating.
namenode02: WARNING: /home/ubuntu/hadoop/logs does not exist. Creating.
Starting resourcemanager
Starting nodemanagers

```
실행결과 이미지


## 실행확인

```
jps
```
namenode01
```
ubuntu@namenode01:~$ jps
3130 Jps
2842 ResourceManager
2558 NameNode
```
namenode02
```
ubuntu@namenode02:~$ jps
2182 Jps
2136 SecondaryNameNode
```
datanode01, datanode02
```
ubuntu@datanode01:~$ jps
2084 DataNode
2250 NodeManager
2346 Jps
```

브라우저에 아래 주소로 접속하면 상태를 확인할 수 있다.  
```
http://네임노드IP:9870
```

데이터 노드를 두개 사용했으니 2개가 표시된다.


## 데이터 입력


## 문제발생 
데이터 노드 2개를 사용했지만 용량부족으로 추가해야 할듯

http://3.34.91.60:9870/


인스턴스 이미지를 복사해서 추가로 만들어준다  
이렇게 하면 세팅파일을 다시 설정하지 않아도 된다.  
하둡만 다시 설치해주고  
.bashrc 수정  
여러 세팅을 완료하고  

다른 노드에 현재 데이터노드를 추가해준다.  
```
hadoop dfsadmin -refreshNodes
```
```
hadoop balancer -threshold 10  
# -threshold 뒤의 숫자는 데이터 노드간 데이터 사이즈 차이의 한계 퍼센티지이다.
# 디폴트 값은 10이라고 한다.  
```

## 파일 시스템 명령어

파일 확인
```
hdfs dfs -ls <dir>

```
파일추가  
```
hdfs dfs -put <파일경로> <추가할경로>
```

## 스파크 설치

https://hoyy.github.io/posts/spark-start-install-standalone

다운로드
```
$ wget https://dlcdn.apache.org/spark/spark-3.4.0/spark-3.4.0-bin-hadoop3.tgz
```

```
$ tar -xf spark-3.4.0-bin-hadoop3.tgz
```

```
$ mv spark-3.4.0-bin-hadoop3 spark-3.4.0
```

```
# Spark
export SPARK_HOME=/home/ubuntu/spark-3.4.0
export PATH=$PATH:$SPARK_HOME/bin:$SPARK_HOME/sbin
export MASTER=spark://spark-master:7077
```

```
source ~/.bashrc
```
아래 명령어를 입력했을 때  
welcome to spark 가 나오면 잘 설치가 된 것이다.  
```
spark-submit --version
```




```
spark-shell
```

```
cd spark-3.4.0/conf
cp spark-env.sh.template spark-env.sh
cp spark-defaults.conf.template spark-defaults.conf
```

```
nano spark-env.sh

export SPARK_DIST_CLASSPATH=$(~/hadoop-3.4.0/bin/hadoop classpath)

export SPARK_MASTER_IP='192.168.80.128'
export SPARK_MASTER_HOST=spark-master

# 안해도됨
export SPARK_MASTER_WEBUI_PORT=7777
export SPARK_WORKER_INSTANCES=2 # 워커 몇개 쓸래? 기본값은 1
export SPARK_EXECUTOR_CORES=1
export SPARK_EXECUTOR_MEMORY=1g
```

```
$ nano spark-defaults.conf

spark.master        spark://spark-master:7077
spark.serializer    org.apache.spark.serializer.KryoSerializer
```

```
$ sudo nano /etc/hosts

<master ip> spark-master
```


```
~/spark-3.4.0/sbin/start-all.sh
```

```


# 마스터에서
~/spark-3.4.0/sbin/start-master.sh
# 슬레이브에서
~/spark-3.4.0/sbin/start-slave.sh ${MASTER}
```

