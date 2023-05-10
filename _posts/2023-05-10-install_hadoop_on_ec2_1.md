---
published: False
title:  "[hadoop, AWS] EC2에 하둡 분산 환경 구축하기 1"
categories: [hadoop, AWS]
tag: [hadoop, AWS, EC2]
---

## 문제정의  

데이터 분석을 위해 사용할 데이터가 20gb가 넘어 in-memory 에서 사용하기 다소 힘들었음.  

## 계획  
하둡을 사용해 분산처리 시스템을 만들고 spark로 분석을 진행해본다.  
ec2 5대를 사용해 하둡 환경을 구축하고 데이터를 처리할 예정

## 참고

이 글은 아래 빅공잼님의 '빅데이터 분석 환경 구축' 영상을 제 환경에 맞춰 글로 옮긴 것입니다.  
영상에 좀 더 자세한 설명이 있으니 보고 따라하셔도 됩니다.  
https://youtube.com/playlist?list=PLJlUnZ1kDbt7X2C4ntIYHmphNDIc5wN8J


## 인스턴스 생성

EC2 인스턴스를 생성한다.  
ubuntu 20.04 LTS, t2.micro 로 생성했다. 

영상에서는 t2.xlarge 로 만들었지만 

인스턴스 생성방법은 따로 설명하지 않는다.  
필요하면 아래 글을 참고하면 된다.


## ssh keygen

접속이 편리하도록 ssh 키를 생성한다.
```
ssh-keygen -t rsa
```
질문이 나오면 그냥 엔터를 쳐서 기본 값으로 생성한다.  
생성되는 경로는 ```~/.ssh/```이다.  

해당경로에 ```config```파일을 만든다.  
```
vim config
```

```
Host nn1
	HostName <네임노드의 퍼블릭 아이피>
	User ubuntu
	IdentityFile <pem 파일 경로>
```

아래 내용을 작성후 저장하고
접속 테스트를 해본다. 

```
ssh nn1
```

## 자바 설치

hadoop yarn spark zookeeper는 자바에서 동작하기 때문에 설치가 필요하다.  

```
sudo apt-get -y update
sudo apt-get -y upgrade
sudo apt-get -y dist-upgrade # 신규 패키지 업그레이드
```

필요 라이브러리 설치
```
sudo apt-get install -y vim wget unzip ssh openssh-* net-tools
```


자바 설치
```
sudo apt-get install -y openjdk-8-jdk
```
자바 설치된 경로
```
/usr/lib/jvm/java-8-openjdk-amd64
```
환경변수 설정
```
sudo vim /etc/environment
```

```PATH``` 뒤에 ```:<자바경로>``` 를 추가하고
```JAVA_HOME="<자바경로>"```를 추가한다. 
```
PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/usr/lib/jvm/java-8-openjdk-amd64/bin"

JAVA_HOME="/usr/lib/jvm/java-8-openjdk-amd64"
```
활성화
```
source /etc/environment
```

```.bashrc```에 ```JAVA_HOME```추가
```
sudo echo 'export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64' >> ~/.bashrc
```
활성화
```
source ~/.bashrc
```

## 하둡 설치

경로생성 및 이동 (필수아님)
```
sudo mkdir /install_dir && cd /install_dir
```

다운로드
```
wget https://dlcdn.apache.org/hadoop/common/hadoop-3.3.5/hadoop-3.3.5.tar.gz
```
압축풀기
```-C``` 명령어를 이용해 ```/usr/local/``` 경로에 압축을 푼다.
꼭 경로를 정해야 할 필요는 없고 원하는 곳에 해도 된다.  

```
sudo tar -zxvf hadoop-3.3.5.tar.gz -C /usr/local
```

폴더명 수정  
매번 ```hadoop-3.3.5```이렇게 쓰기 귀찮기 때문에 편의상 바꾸는 것이고 필수는 아니다.  
이 글은 폴더명을 바꾸는 것을 전제로 쓰여있기 때문에 바꾸는 편이 덜 귀찮을 것이다.  

```
sudo mv /usr/local/hadoop-3.3.5 /usr/local/hadoop
```

환경변수 설정

자바 설치때 처럼 환경변수를 설정한다.  
```PATH``` 뒤에 ```:/usr/local/hadoop/bin```과 ```:/usr/local/hadoop/sbin``` 
두가지를 추가하고  
```HADOOP_HOME="/usr/local/hadoop"``` 도 추가한다.  

```
sudo vim /etc/environment

PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/usr/lib/jvm/java-8-openjdk-amd64/bin:/usr/local/hadoop/bin:/usr/local/hadoop/sbin"

JAVA_HOME="/usr/lib/jvm/java-8-openjdk-amd64"
HADOOP_HOME="/usr/local/hadoop"                  
```

활성화
```
source vim /etc/environment
```

### 사용자 환경변수 설정

환경변수 추가
```
sudo echo 'export HADOOP_HOME=/usr/local/hadoop' >> ~/.bashrc
sudo echo 'export HADOOP_COMMON_HOME=$HADOOP_HOME' >> ~/.bashrc
sudo echo 'export HADOOP_HDFS_HOME=$HADOOP_HOME' >> ~/.bashrc
sudo echo 'export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop' >> ~/.bashrc
sudo echo 'export YARN_CONF_DIR=$HADOOP_HOME/etc/hadoop' >> ~/.bashrc
sudo echo 'export HADOOP_YARN_HOME=$HADOOP_HOME' >> ~/.bashrc
sudo echo 'export HADOOP_MAPRED_HOME=$HADOOP_HOME' >> ~/.bashrc
```

활성화
```
source ~/.bashrc
```

확인
```
env | grep HADOOP
```

### hdfs-site.xml
```
sudo vim $HADOOP_HOME/etc/hadoop/hdfs-site.xml

<configuration>
    <!-- configuration hadoop -->
    <property>
        <name>dfs.replication</name>
        <value>2</value>
    </property>
    <property>
        <name>dfs.namenode.name.dir</name>
        <value>/usr/local/hadoop/data/nameNode</value>
    </property>
    <property>
        <name>dfs.datanode.data.dir</name>
        <value>/usr/local/hadoop/data/dataNode</value>
    </property>
    <property>
        <name>dfs.journalnode.edits.dir</name>
        <value>/usr/local/hadoop/data/dfs/journalnode</value>
    </property>
    <property>
        <name>dfs.nameservices</name>
        <value>my-hadoop-cluster</value>
    </property>
    <property>
        <name>dfs.ha.namenodes.my-hadoop-cluster</name>
        <value>namenode1,namenode2</value>
    </property>
    <property>
        <name>dfs.namenode.rpc-address.my-hadoop-cluster.namenode1</name>
        <value>nn1:8020</value>
    </property>
    <property>
        <name>dfs.namenode.rpc-address.my-hadoop-cluster.namenode2</name>
        <value>nn2:8020</value>
    </property>
    <property>
        <name>dfs.namenode.http-address.my-hadoop-cluster.namenode1</name>
        <value>nn1:50070</value>
    </property>
    <property>
        <name>dfs.namenode.http-address.my-hadoop-cluster.namenode2</name>
        <value>nn2:50070</value>
    </property>
    <property>
        <name>dfs.namenode.shared.edits.dir</name>
        <value>qjournal://nn1:8485;nn2:8485;dn1:8485/my-hadoop-cluster</value>
    </property>
    <property>
        <name>dfs.client.failover.proxy.provider.my-hadoop-cluster</name>
        <value>org.apache.hadoop.hdfs.server.namenode.ha.ConfiguredFailoverProxyProvider</value>
    </property>
    <property>
        <name>dfs.ha.fencing.methods</name>
        <value>shell(/bin/true)</value>
    </property>
    <property>
        <name>dfs.ha.fencing.ssh.private-key-files</name>
        <value>/home/ubuntu/.ssh/id_rsa</value>
    </property>
    <property> 
        <name>dfs.ha.automatic-failover.enabled</name>
        <value>true</value>
    </property>
    <property>
        <name>dfs.name.dir</name>
        <value>/usr/local/hadoop/data/name</value>
    </property>
    <property>
        <name>dfs.data.dir</name>
        <value>/usr/local/hadoop/data/data</value>
    </property>
</configuration>
```

### core-site.xml

```
sudo vim $HADOOP_HOME/etc/hadoop/core-site.xml

<configuration>
        <property>
                <name>fs.default.name</name>
                <value>hdfs://nn1:9000</value>
        </property>
        <property>
                <name>fs.defaultFS</name>
                <value>hdfs://my-hadoop-cluster</value>
        </property>
        <property>
                <name>ha.zookeeper.quorum</name>
                <value>nn1:2181,nn2:2181,dn1:2181</value>
        </property>
</configuration>
```

### yarn-site.xml
```
sudo vim $HADOOP_HOME/etc/hadoop/yarn-site.xml

<configuration>
        <!-- Site specific YARN configuration properties -->
        <property>
                <name>yarn.nodemanager.aux-services</name>
                <value>mapreduce_shuffle</value>
        </property>
        <property>
                <name>yarn.nodemanager.aux-services.mapreduce_shuffle.class</name>
                <value>org.apache.hadoop.mapred.ShuffleHandler</value>
        </property>
        <property>
                <name>yarn.resourcemanager.hostname</name>
                <value>nn1</value>
        </property>
        <property>
                <name>yarn.nodemanager.vmem-check-enabled</name>
                <value>false</value>
        </property>
</configuration>
```

### mapred-stie.xml

```
sudo vim $HADOOP_HOME/etc/hadoop/mapred-site.xml

<configuration>
        <property>
                <name>mapreduce.framework.name</name>
                <value>yarn</value>
        </property>
        <property>
                <name>yarn.app.mapreduce.am.env</name>
                <value>HADOOP_MAPRED_HOME=${HADOOP_HOME}</value>
        </property>
        <property>
                <name>mapreduce.map.env</name>
                <value>HADOOP_MAPRED_HOME=${HADOOP_HOME}</value>
        </property>
        <property>
                <name>mapreduce.reduce.env</name>
                <value>HADOOP_MAPRED_HOME=${HADOOP_HOME}</value>
        </property>
</configuration>
```

### hadoop-env.sh

```
sudo vim $HADOOP_HOME/etc/hadoop/hadoop-env.sh

# 아래에

# Java
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64

# Hadoop
export HADOOP_HOME=/usr/local/hadoop
```

### worker master

워커를 설정한다.  
```
sudo vim $HADOOP_HOME/etc/hadoop/workers
```

localhost 를 주석처리 하고 아래와 같이 작성
```
# localhost
dn1
dn2
dn3
```


마스터를 설정한다.  
```
sudo vim $HADOOP_HOME/etc/hadoop/master
```

```
nn1
nn2
```

## 스파크 설치

인스톨 경로에서 시작
```
cd /install_dir
```

스파크 다운
```
sudo wget https://dlcdn.apache.org/spark/spark-3.4.0/spark-3.4.0-bin-hadoop3.tgz
```

압축풀기
```
sudo tar -xzvf spark-3.4.0-bin-hadoop3.tgz -C /usr/local
```

폴더 이름 변경
```
sudo mv /usr/local/spark-3.4.0-bin-hadoop3 /usr/local/spark
```

### 라이브러리 설치

파이썬 설치
```
sudo apt-get install -y python3-pip
```

파이스파크, 파인드스파크 설치
```
sudo pip3 install pyspark findspark
```

### 환경변수 설정
```
sudo vim /etc/environment
```

```PATH``` 뒤에 ```:/usr/local/spark/bin```과 ```:/usr/local/spark/sbin``` 
두가지를 추가하고  
```SPARK_HOME="/usr/local/spark"``` 도 추가한다.  

```
PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/usr/lib/jvm/java-8-openjdk-amd64/bin:/usr/local/hadoop/bin:/usr/local/hadoop/sbin"

JAVA_HOME="/usr/lib/jvm/java-8-openjdk-amd64"
HADOOP_HOME="/usr/local/hadoop"
SPARK_HOME="/usr/local/spark"
```
활성화
```
source /etc/environment
```

사용자 환경변수 추가
```
echo 'export SPARK_HOME=/usr/local/spark' >> ~/.bashrc
```
활성화
```
source ~/.bashrc
```
적용 확인
```
env | grep SPARK
```

### 환경설정 파일 작성

```
cd $SPARK_HOME/conf
```

```spark-env.sh``` 파일 작성
```
sudo cp spark-env.sh.template spark-env.sh
```
```
sudo vim spark-env.sh
```
아래에 추가
```
export SPARK_HOME=/usr/local/spark
export SPARK_CONF_DIR=/usr/local/spark/conf
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export HADOOP_HOME=/usr/local/hadoop
export HADOOP_CONF_DIR=/usr/local/hadoop/etc/hadoop
export SPARK_MASTER_WEBUI_PORT=18080
```

```spark-defaults.conf``` 파일 작성
```
sudo cp spark-defaults.conf.template spark-defaults.conf
```
```
sudo vim spark-defaults.conf
```
아래에 추가
```
spark.master            yarn
spark.eventLog.enabled  true
spark.eventLog.dir      /usr/local/spark/logs
```
로그 디렉토리 생성
```
sudo mkdir -p /usr/local/spark/logs
```
로그 디렉토리 소유권 수정
```
sudo chown -R $USER:$USER /usr/local/spark/
```

workers 파일 편집
```
cd /usr/local/spark/conf
```
```
sudo cp workers.template workers
```
```
sudo vim workers
```
```localhost```를 주석하고 아래 내용 추가  
```
# localhost
dn1
dn2
dn3
```

### 파이썬 환경설정

```
sudo vim /etc/environment
```
```PATH```뒤에 ```/usr/bin/python3```추가
```
PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/usr/lib/jvm/java-8-openjdk-amd64/bin:/usr/local/hadoop/bin:/usr/local/hadoop/sbin:/usr/bin/python3"
```
활성화
```
source /etc/environment
```
사용자 환경변수 추가
```
sudo echo 'export PYTHONPATH=/usr/bin/python3' >> ~/.bashrc
sudo echo 'export PYSPARK_PYTHON=/usr/bin/python3' >> ~/.bashrc
```
활성화
```
source ~./bashrc
```
확인
```
env | grep PYTHON
```

## 주키퍼 설치

```
cd /install_dir
```
다운로드
```
sudo wget https://dlcdn.apache.org/zookeeper/zookeeper-3.8.1/apache-zookeeper-3.8.1-bin.tar.gz
```
압축풀기
```
sudo tar -xzvf apache-zookeeper-3.8.1-bin.tar.gz -C /usr/local
```
폴더명 수정
```
sudo mv /usr/local/apache-zookeeper-3.8.1-bin /usr/local/zookeeper
```

환경변수 설정
```
sudo vim /etc/environment
```
```ZOOKEEPER_HOME```추가
```
ZOOKEEPER_HOME="/usr/local/zookeeper"
```
활성화
```
source /etc/environment
```

유저 환경변수 추가
```
echo 'export ZOOKEEPER_HOME=/usr/local/zookeeper' >> ~/.bashrc
```
활성화
```
source ~/.bashrc
```
확인
```
env | grep ZOOKEEPER
```

### 주키퍼 환경 설정

주키퍼 경로로 이동
```
cd $ZOOKEEPER_HOME
```
```zoo.cfg``` 파일 생성
```
sudo cp ./conf/zoo_sample.cfg ./conf/zoo.cfg
```
```
sudo vim ./conf/zoo.cfg
```

```dataDir``` 주석처리 하고 새 경로 설정.
```dataLogDir```추가
```
# dataDir=/tmp/zookeeper
dataDir=/usr/local/zookeeper/data
dataLogDir=/usr/local/zookeeper/logs
```

기타 설정값 추가
```
maxClientCnxns=0
maxSessionTimeout=180000
server.1=nn1:2888:3888
server.2=nn2:2888:3888
server.3=dn1:2888:3888
```

설정한 디렉토리 생성
```
sudo mkdir -p /usr/local/zookeeper/data
sudo mkdir -p /usr/local/zookeeper/logs
```
소유권 변경
```
sudo chown -R $USER:$USER /usr/local/zookeeper
```

myid 생성 하고 내용은 ```1```을 넣어준다.
```
sudo vim /usr/local/zookeeper/data/myid
```
```
1
```

### ssh 키 생성

```
ssh-keygen -t rsa
```
다음 나오는 질문3 가지는 전부 엔터친다.  
```
Enter file in which to save the key (/home/ubuntu/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
```

생성 확인
```
ls -al ~/.ssh
```

!이미지 삽입

인증키 추가
```
cat >> ~/.ssh/authorized_keys < ~/.ssh/id_rsa.pub
```
접속 확인
```
ssh localhost
```

