---
published: True
title:  "[Anaconda, pip] requirements 내보내기/불러오기"
categories: Etc
tag: [requirements, conda, pip, anaconda]
---

## CONDA 환경

Requiremnts.txt 생성  
```
conda env export > conda_requirements.yaml  
```
Requiremnts.txt 와 동일한 환경 설치  
```
conda env create -f conda_requirements.yaml
```

conda list 저장하기(export)  
```
conda list --export > conda_requirements.txt
```
conda list 불러오기(Import)  
새로운 가상환경에서 라이브러리 인스톨  
```
conda install --file conda_requirements.txt
```

## PIP 환경

Requiremnts.txt 생성  
```
pip freeze > requirements.txt
```

Requiremnts.txt 와 동일한 환경 설치  
```
pip install -r requirements.txt
``` 

### 참고

<https://1ch0.tistory.com/6>  
<https://keepdev.tistory.com/27>