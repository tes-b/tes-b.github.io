---
published: True
title:  "[AWS] scp로 EC2 인스턴스에 파일 전송"
categories: AWS
tag: [AWS, EC2, SSH]
---


scp로 AWS EC2 인스턴스로 파일 업로드/다운로드 하는 방법

## 업로드
```
scp -i [pem파일경로] [업로드할 파일 이름] [ec2-user계정명]@[ec2 instance의 public DNS]:~/[경로]
```

```
ex)

scp -i "my_key_0.pem" /home/tesb/Documents/sending_test.py ubuntu@ec2-3-35-148-232.ap-northeast-2.compute.amazonaws.com:~/
```

테스트  

![Image0](/images/2023-01-15-scp_send_file_0.png)*전송이 잘 되었다.*   


## 다운로드
```
scp -i [pem파일경로] [ec2-user계정명]@[ec2 instance의 public DNS]:~/[경로] [다운로드 파일의 로컬 경로] 
```
```
ex)

scp -i "my_key_0.pem" ubuntu@ec2-3-35-148-232.ap-northeast-2.compute.amazonaws.com:~/downloading_test.py /home/tesb/Documents/ 
```

### 참고

<https://ict-nroo.tistory.com/40>