---
published: True
title:  "[Anaconda] 특정 컬럼이 포함된 테이블 찾기"
categories: [Anaconda]
tag: [Anaconda, python, Error]
---

## 문제
```
CondaSSLError: Encountered an SSL error. Most likely a certificate verification issue. Exception: HTTPSConnectionPool(host='repo.anaconda.com', port=443): Max retries exceeded with url: /pkgs/main/win-64/repodata.json.zst (Caused by SSLError(SSLCertVerificationError(1, '[SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed: self-signed certificate in certificate chain (_ssl.c:1000)')))
```  


아나콘다 가상환경 생성시 위와 같은 에러가 뜨는 경우  
회사가 프록시를 사용하거나 혹은 자기가 사용하고 있어서 그렇다.  
  

## 해결방법

둘중 하나를 사용하면된다.

1. SSL configuration을 끈다.
```
conda config --set ssl_verify false
```

OR

인증서가 있을 경우 세팅한다.  

2. certificate 를 추가한다.
```
conda config --set ssl_verify <인증서경로>.crt
```