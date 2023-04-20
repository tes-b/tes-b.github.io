---
published: True
title:  "[Django] iframe 127.0.0.1 refused to connect"
categories: [Django]
tag: [Django, error]
---

## 문제

장고로 웹사이트 제작 중 iframe을 사용해서 다른 페이지를 띄우려고 했다.  
```html
<iframe class="overlay" id="wordle" src="/wordle/"></iframe>
```

그런데 아래 스크린 샷 처럼 뜨면서 페이지가 나오지 않는다.  

![image0](/images/2023-03-24-django_refused_to_connect_0.png)*스크린샷*  


## 해결 방법

chatGPT에 물어봤다.  

> 미들웨어가 있는 경우 Django 프로젝트는 HTTP 응답에서 X-Frame-Options 헤더를 설정하도록 구성됩니다. 기본값은 거부입니다. settings.py 파일에 다음 줄을 추가하여 값을 SAMEORIGIN으로 변경하여 동일한 도메인의 iframe 내에 사이트를 삽입할 수 있습니다.

라고 한다.  

그래서 시키는대로 ```X_FRAME_OPTIONS = 'SAMEORIGIN'``` 을  
```settings.py```에 추가하니 잘 동작했다.  

```py
# settings.py

MIDDLEWARE = [
    # ...
    "django.middleware.clickjacking.XFrameOptionsMiddleware",
    # ...
]

X_FRAME_OPTIONS = 'SAMEORIGIN'
```

### 여담
이런거 포스팅해도 검색하는 사람있으려나? GPT한테 물어보면 되는데.  