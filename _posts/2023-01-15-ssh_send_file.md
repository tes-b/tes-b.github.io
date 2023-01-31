---
published: False
title:  "[AWS] ssh로 EC2 인스턴스에 파일 전송"
categories: AWS
tag: [AWS, EC2, SSH]
---


ssh로 AWS EC2 인스턴스로 파일 업로드/다운로드 하는 방법

## 업로드
```
scp -i [pem파일경로] [업로드할 파일 이름] [ec2-user계정명]@[ec2 instance의 public DNS]:~/[경로]
```

```
ex)

scp -i "my_key.pem" file_send.py 
ubuntu@ec2~~~~~~.ap-northeast-2.compute.amazonaws.com:~/test/
```

## 다운로드
```
scp -i [pem파일경로] [ec2-user계정명]@[ec2 instance의 public DNS]:~/[경로] [다운로드 파일의 로컬 경로] 
```
```
ex)

scp -i "D:/document/aws/keypair/namjunemy_key_pair_ldcc_notebook.pem"
ubuntu@ec2~~~~~~.ap-northeast-2.compute.amazonaws.com:~/test/test.txt D:/document/
```

### 참고

https://ict-nroo.tistory.com/40