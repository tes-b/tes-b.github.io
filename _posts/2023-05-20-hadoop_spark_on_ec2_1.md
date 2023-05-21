---
published: true
title:  "[hadoop, spark, AWS] EC2에 하둡 스파크 클러스터 구축하기 - 1"
categories: [hadoop, spark, AWS]
tag: [hadoop, spark, AWS, EC2, zookeeper]
---

AWS 환경에서 하둡 스파크 클러스터를 구축하는 과정을 기록했다.  

2편: <https://tes-b.github.io/hadoop/spark/aws/hadoop_spark_on_ec2_2/>  
3편: <https://tes-b.github.io/hadoop/spark/aws/hadoop_spark_on_ec2_3/>  


이 글은 빅공잼님의 '빅데이터 분석 환경 구축' 영상을 보고  
직접 설치해 보면서 제 환경에 맞춰 글로 옮긴 것이며  
개인적인 기록용도로 작성하는 것이니 설명이 생략된 부분이 많다.  
EC2와 우분투에 대해 조금은 알고 있어야 이해가 편할 것이다.  

빅공잼님의 영상은 좀 더 자세한 설명이 있으니 보고 따라해도 된다.  
<https://youtube.com/playlist?list=PLJlUnZ1kDbt7X2C4ntIYHmphNDIc5wN8J>


### 주의사항

나는 영상과 다르게 하둡, 스파크, 제플린 모두 최신버전으로 설치했는데  
제플린과 스파크의 버전 충돌로 파이스파크 실행은 실패했다.  
내 경우 주피터를 올려서 파이스파크를 사용했지만  
제플린에서 파이스파크를 실행하려면 버전을 잘 확인해서 설치하거나  
영상과 같은 버전을 설치하는게 좋다.    

## 설치 버전

이 포스트 (제플린에서 파이스파크 실행 안됨)
- Apache Hadoop 3.3.5
- Apache Spark 3.4.0 
- Apache Zookeeper 3.8.1
- Zeppelin 0.10.1

원본 영상
- Apache Hadoop 3.2.3
- Apache Spark 3.2.1 
- Apache Zookeeper 3.8.0
- Zeppelin 0.10.1


## 인스턴스 생성

EC2 인스턴스를 생성한다.  
인스턴스 1개만 생성한뒤 필요한 설치를 진행하고  
AMI를 만들어 4개를 추가로 복제한 뒤 각각 필요한 세팅을 진행할 것이다.  

**EC2 세팅**

- 운영체제 : ubuntu 20.04 LTS  
- 인스턴스 타입 : t2.xlarge  
- 스토리지 : 30gb  

**주의할 점**

처음에는 ```t2.micro``` 인스턴스를 선택해 진행했으나  
실행되는 프로세스가 많아 서버가 터지는 일이 발생했다.  
원활한 진행을 위해서는 ```t2.large``` 이상의 인스턴스로 진행하는 것을 추천한다.  

## ssh keygen

로컬에서 ssh로 서버에 접속할 것이다.  
ssh 키를 생성한다.  

```
ssh-keygen -t rsa
```
<br>
질문이 3가지가 나오는데 나오면 그냥 엔터를 쳐서 기본 값으로 생성한다.  
생성되는 경로는 ```~/.ssh/```이다.  

그다음 ```config```파일을 만들고  
```
vim ~/.ssh/config
```
다음을 추가한다.
```
# ~/.ssh/config >

Host nn1
	HostName <네임노드의 퍼블릭 아이피>
	User ubuntu
	IdentityFile <pem 파일 경로>
```
저장하고 접속 테스트를 해본다. 

```
ssh nn1
```
아래 질문에는 ```yes```를 입력하면 된다. 
```
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

접속이 된다면 잘 설정이 된 것이다.  
<br>

## 자바 설치

hadoop, yarn, spark, zookeeper는 자바에서 동작하기 때문에 설치가 필요하다.  

```
sudo apt-get -y update
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

```PATH``` 맨 뒤에 구분자 ```:```를 넣고  
```/usr/lib/jvm/java-8-openjdk-amd64``` 를 추가하고  
아래에 ```JAVA_HOME="/usr/lib/jvm/java-8-openjdk-amd64"```를 추가한다.  
```
# /etc/environment >

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

**경로생성 및 이동**
```
sudo mkdir /install_dir && cd /install_dir
```
다운로드 받은 파일들을 전부 여기에 모을 예정이다.  
<br>

파일 다운로드  

<https://hadoop.apache.org/>  
하둡 홈페이지에 가서 원하는 버전의 링크를 가져오자
```
wget https://dlcdn.apache.org/hadoop/common/hadoop-3.3.5/hadoop-3.3.5.tar.gz
```

압축풀기  

```-C``` 명령어를 이용해 ```/usr/local/``` 경로에 압축을 푼다.  

이 포스트는 ```/usr/local/```경로를 기준으로 작성되었으나  
원한다면 다른 경로에 해도 상관없다.

```
sudo tar -zxvf hadoop-3.3.5.tar.gz -C /usr/local
```

폴더명 수정  

매번 ```hadoop-3.3.5```이렇게 쓰기 귀찮기 때문에 편의상 바꾸는 것이고 필수는 아니다.  
이 포스트는 폴더명을 바꾸는 것을 기준으로 작성되었으나  
원한다면 그대로 써도 상관없다.  

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
```
```
# /etc/environment >

PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/usr/lib/jvm/java-8-openjdk-amd64/bin:/usr/local/hadoop/bin:/usr/local/hadoop/sbin"

JAVA_HOME="/usr/lib/jvm/java-8-openjdk-amd64"
HADOOP_HOME="/usr/local/hadoop"                  
```

활성화
```
source /etc/environment
```

### 사용자 환경변수 설정

환경변수 추가
```~/.bashrc```에 변수를 추가한다.  
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

적용 확인  
```
env | grep HADOOP
```

hdfs-site.xml 수정  

```
sudo vim $HADOOP_HOME/etc/hadoop/hdfs-site.xml
```
```xml
# $HADOOP_HOME/etc/hadoop/hdfs-site.xml >

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
<br>

core-site.xml 수정  
```
sudo vim $HADOOP_HOME/etc/hadoop/core-site.xml
```
```xml
# $HADOOP_HOME/etc/hadoop/core-site.xml >

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
<br>

yarn-site.xml 수정  
```
sudo vim $HADOOP_HOME/etc/hadoop/yarn-site.xml
```
```xml
# $HADOOP_HOME/etc/hadoop/yarn-site.xml >

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
<br>

mapred-stie.xml 수정  
```
sudo vim $HADOOP_HOME/etc/hadoop/mapred-site.xml
```
```xml
# $HADOOP_HOME/etc/hadoop/mapred-site.xml >

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
<br>

hadoop-env.sh 수정  
```
sudo vim $HADOOP_HOME/etc/hadoop/hadoop-env.sh
```

아래에 다음 내용을 추가한다.  
```
# $HADOOP_HOME/etc/hadoop/hadoop-env.sh >


# Java
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64

# Hadoop
export HADOOP_HOME=/usr/local/hadoop
```
<br>

**worker master 설정**  

워커를 설정한다.  
```
sudo vim $HADOOP_HOME/etc/hadoop/workers
```


```localhost``` 를 주석처리 하고 아래와 같이 작성
```
# $HADOOP_HOME/etc/hadoop/workers >

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
# $HADOOP_HOME/etc/hadoop/master >

nn1
nn2
```

### 소유권 설정
```
sudo chown -R $USER:$USER /usr/local/hadoop/
```
처음엔 소유권 설정을 하지않고 진행했었는데    
이후 하둡 파일 시스템 포맷시 진행이 안되는 문제가 있었음  

## 스파크 설치


다운로드 경로로 이동  
```
cd /install_dir
```

다운로드   
```
sudo wget https://dlcdn.apache.org/spark/spark-3.4.0/spark-3.4.0-bin-hadoop3.tgz
```

압축풀기  
```
sudo tar -xzvf spark-3.4.0-bin-hadoop3.tgz -C /usr/local
```

폴더명 수정  
```
sudo mv /usr/local/spark-3.4.0-bin-hadoop3 /usr/local/spark
```
<br>

### 라이브러리 설치

파이썬 설치
```
sudo apt-get install -y python3-pip
```

파이스파크, 파인드스파크 설치
```
sudo pip3 install pyspark findspark
```
<br>

### 환경변수 설정
```
sudo vim /etc/environment
```

```PATH``` 뒤에 ```:/usr/local/spark/bin```과 ```:/usr/local/spark/sbin``` 
두가지를 추가하고  
```SPARK_HOME="/usr/local/spark"``` 도 추가한다.  

```
# /etc/environment >

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
<br>

### 환경설정 파일 작성

스파크 conf 경로로 이동
```
cd $SPARK_HOME/conf
```

스파크는 스크립트 파일들의 템플릿이 있어  
원하는 파일을 복사해서 사용하면 된다.  

```spark-env.sh``` 파일 작성
```
sudo cp spark-env.sh.template spark-env.sh
```
```
sudo vim spark-env.sh
```

아래에 추가
```
# $SPARK_HOME/conf/spark-env.sh >

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
# $SPARK_HOME/conf/spark-defaults.conf >

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
# /usr/local/spark/conf/workers >

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
# /etc/environment >

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

적용 확인
```
env | grep PYTHON
```
<br>

## 주키퍼 설치

다운로드 경로 이동
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
```
# /etc/environment >

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

적용 확인
```
env | grep ZOOKEEPER
```
<br>

### 주키퍼 환경 설정

주키퍼 경로로 이동
```
cd $ZOOKEEPER_HOME
```

```zoo.cfg``` 파일 생성
```
sudo cp ./conf/zoo_sample.cfg ./conf/zoo.cfg
```

편집
```
sudo vim ./conf/zoo.cfg
```

```dataDir``` 주석처리 하고 새 경로 설정.  
```dataLogDir``` 및 기타 설정값 추가
```
# ./conf/zoo.cfg >

# dataDir=/tmp/zookeeper
dataDir=/usr/local/zookeeper/data
dataLogDir=/usr/local/zookeeper/logs

maxClientCnxns=0
maxSessionTimeout=180000
server.1=nn1:2888:3888
server.2=nn2:2888:3888
server.3=dn1:2888:3888
```

```dataLogDir```추가 한 디렉토리 생성
```
sudo mkdir -p /usr/local/zookeeper/data
sudo mkdir -p /usr/local/zookeeper/logs
```

소유권 변경
```
sudo chown -R $USER:$USER /usr/local/zookeeper
```

```myid```를 생성 하고 내용은 ```1```을 넣어준다.
```
sudo vim /usr/local/zookeeper/data/myid
```
```
# /usr/local/zookeeper/data/myid >

1
```
<br>

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

![Image0](/images/2023-05-20-hadoop_spark_on_ec2_1_0.png)  


인증키 추가
```
cat >> ~/.ssh/authorized_keys < ~/.ssh/id_rsa.pub
```

접속 확인
```
ssh localhost
```
<br>

## 2편에서 계속

여기까지 진행하면 기본 세팅을 끝났다.  
이제 이 인스턴스를 AMI로 이미지를 만들어 복사한뒤   
각각의 인스턴스에 필요한 설정을 해 주면 된다.  

이미지를 만들고 각 세팅을 하는 방법은 2편에서 이어 진행한다.  

2편: <https://tes-b.github.io/hadoop/spark/aws/hadoop_spark_on_ec2_2/>  