---
published: True
title:  "[Django] model DateTimeField 한국 시간 설정"
categories: [Django]
tag: [Django, model, datetime]
---

<https://programmers-sosin.tistory.com/entry/Django-model-DateTimeField-%ED%95%9C%EA%B5%AD-%EC%8B%9C%EA%B0%84%EC%9C%BC%EB%A1%9C-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0>

내가 필요할때 찾아보려고 위 블로그의 글을 그대로 옮겼다.  

## 문제

```py
time = models.DateTimeField(auto_now=True, null=True)
```

TIME_ZONE = 'Asia/Seoul' 로 설정했지만  
위의 모델에서 시간이 UTC로 저장이 됨.  

## 해결방법

settings의 ```USE_TZ``` 값을 ```False```로 바꿔주면 된다.

```py
# settings.py

TIME_ZONE = "Asia/Seoul"

USE_I18N = True

USE_TZ = False
```

