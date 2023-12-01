---
published: True
title:  "[git] tificate problem:: self-signed certificate in certificate chain"
categories: [git, github]
tag: [git, github]
---

## 문제 
깃허브 레포지토리를 클론 하려는데 다음과 같은 에러가 발생했다.  

```tificate problem:: self-signed certificate in certificate chain```  

찾아보니 연결 시도한 깃 서버에서 제공된 SSL 인증서를 깃 클라이언트가 신뢰하지 않아서 그렇다고 한다.  
인증서가 self-signed이거나 신뢰할 수 없는 인증기관에서 서명된 경우 발생할수 있다고 나온다.

## 해결방법

인증 절차를 무시하는 방법.

```
git config --global http.sslVerify false
```

![Image1](/images/2023-12-01-git_self_signed_certificate_01.png)

### 출처  

<https://confluence.atlassian.com/bbkb/ssl-certificate-problem-self-signed-certificate-in-certificate-chain-error-in-git-1224773006.html>  
<https://swjeong.tistory.com/161>  