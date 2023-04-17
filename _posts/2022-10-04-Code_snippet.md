---
published: True
title:  "[VSCode] 코드 스니펫(snippet) 관리하기"
categories: [VSCode]
tag: [VSCode, snippet]
---

## 코드 스니펫(snippet) 이란?

>스니펫(snippet)은 재사용 가능한 소스 코드, 기계어, 텍스트의 작은 부분을 일컫는   
프로그래밍 용어이다. 사용자가 루틴 편집 조작 중 반복 타이핑을 회피할 수 있게 도와준다.  
\- 위키백과 -

설명처럼 코드 조각을 말한다.  
자주 사용하거나 범용성 있는 짧은 코드 조각을 만들어 두고 필요할 때 쓱 꺼내서 사용하면 된다.  
메모장이나 기타 텍스트 에디터를 사용해 모아두고 사용할 수도 있지만  
VSCode에는 이것을 따로 관리 할 수 있는 편리한 기능이 존재한다.  

## VSCode configue user snippet 사용하기


File - Preferences - Configure User Snippets  

![image](/images/2022-10-04-Code_snippet_0.png)  

python 선택  

![image](/images/2022-10-04-Code_snippet_1.png)  


```json
	"스니펫 이름": {
		"prefix": "불러올때 사용할 이름",
		"body": [
            // 내용
		],
		"description": "설명"
	},
```
기본 구성은 이렇게 됩니다.  


자주 사용하는 라이브러리를 임포트하는 스니펫을 작성해 보겠습니다.  

```json
	// Modules
	"snp_modules_basic": {
		"prefix": "snp_modules_basic",
		"body": [
			"import pandas as pd",
			"import numpy as np",
			"import matplotlib.pyplot as plt",
			"import seaborn as sns",
		],
		"description": "show_correlation_in_descending"
	},
```
panda, numpy 등등 자주쓰는 라리브러리 임포트 코드를 작성했습니다.  

## 사용하기

불러올 이름인 "snp_modules_basic"을 작성하고 엔터를 치면 자동으로 코드가 완성됩니다!  

![image](/images/2022-10-04-Code_snippet_2.png)*자동완성이 뜬다.*  

![image](/images/2022-10-04-Code_snippet_3.png)*자동으로 코드가 완성된다.*  

