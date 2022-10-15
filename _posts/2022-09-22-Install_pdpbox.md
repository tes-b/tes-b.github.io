---
published: true
title:  "pdpbox 설치 에러와 해결 방법"
categories: Etc
tag: [VSCdoe, pdpbox, python]
---
## pdpbox 설치 에러

pdpbox 라이브러리를 사용하기 위해 설치를 시도했는데  
다음과같은 오류가 뜨면서 설치가 되지 않았다.  

<span style="color:red">
ERROR: Could not install packages due to an OSError: [WinError 5] Access is denied:  
Consider using the `--user` option or check the permissions.
</span>  

설명대로 '--user'를 붙여 관리자 권한으로 설치를 해봐도 설치가 안되는 것은 마찬가지...  

이유를 찾아보니 pdpbox가 최신버전 파이썬 과 호환이 되지 않아서 그렇다고 한다.  

아나콘다를 이용해 python 3.6 버전의 가상환경을 만들고 pdpbox를 설치하면 정상적으로 설치가 된다.  

## 해결방법
아나콘다로 python 3.6 버전의 가상관경을 만든뒤 활성화해준다.
```
(base) C:\Windows\system32>conda activate py36_pdpbox
```
그 다음 아무것도 설치하지 않고 바로 pdpbox를 설치하면 아나콘다가 알아서 matplotlib, pandas 등의 필요한 라이브러리들을 호환되는 버전으로 설치해 준다! (아나콘다 최고...)
```
(py36_pdpbox) C:\Windows\system32>pip install pdpbox
```
설치가 완료되면 원하는 IDE와 연동해 사용하면 된다!


