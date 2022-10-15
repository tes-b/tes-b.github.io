---
published: true
title:  "위키피디아(wikipedia) 페이지 크롤링"
categories: python
tag: [wikipedia, crawling, scraping, data]
---

위키피디아(wikipeida)에는 페이지를 손쉽게 크롤링 할수 있도록 api를 제공한다.

## 설치

사용전에 먼저 api를 설치해줘야 한다.  
IntEnum 을 사용하기 때문에 파이썬 3.4 버전 이상이 필요하다고 한다.  

터미널에서 아래 코드로 api를 설치한다.

```
pip install wikipedia-api
```

## 임포팅

설치가 되었으면 api를 임포트 한다.
```py
import wikipediaapi
```

## 단일 페이지 가져오기

위키피디아 인스턴스를 만든 후 파이썬 페이지를 가져오는 방법이다.

```py
    wiki_wiki = wikipediaapi.Wikipedia('en');
    page_py = wiki_wiki.page('Python_(programming_language)');
```
인스턴스 생성시 'en' 파라미터는 영문 위키를 가져온다는 뜻이다.  
'kr'로 한국어 위키도 가져올 수 있다.

```page("원하는 페이지 제목")``` 으로 페이지를 가져올 수 있다.

## 페이지 존재 확인

페이지가 존재하면 True, 아니면 False를 반환한다.
```py
    page_py = wiki_wiki.page('Python_(programming_language)');
    print(f"Page - Exists: {page_py.exists()}");
    # Page - Exists: True

    page_missing = wiki_wiki.page('NonExistingPageWithStrangeName');
    print(f"Page - Exists: {page_missing.exists()}");
    # Page - Exists: False
```

## 페이지 제목과 요약 가져오기

페이지 제목과 가장위의 요약 문단을 가져온다.
```py
    print(f"Page - Title: {page_py.title}");
    # Page - Title: Python (programming language)
    print(f"Page - Summary: {page_py.summary}");
    # Page - Summary: Python is a widely used high-level programming...    
```
## 페이지 url 가져오기
```py
    print(page_py.fullurl);
    # https://en.wikipedia.org/wiki/Python_(programming_language)

    print(page_py.canonicalurl);
    # https://en.wikipedia.org/wiki/Python_(programming_language)
```
## 전체 페이지 가져오기

```py
wiki_wiki = wikipediaapi.Wikipedia(
        language='en',
        extract_format=wikipediaapi.ExtractFormat.WIKI);

p_wiki = wiki_wiki.page("Python_(programming_language)");
print(p_wiki.text);
# Summary
# Section 1
# Text of section 1
# Section 1.1
# Text of section 1.1
# ...
```
```extract_format```을 ```wikipediaapi.ExtractFormat.HTML```로 지정시  
HTML 포멧으로도 가져올 수 있다.

## 섹션 가져오기

가져온 페이지에서 섹션별로 가져온다.
```py
def print_sections(sections, level=0):
        for s in sections:
                print("%s: %s - %s" % ("*" * (level + 1), s.title, s.text[0:40]));
                print_sections(s.sections, level + 1);

print_sections(page_py.sections);

# *: History - Python was conceived in the late 1980s,
# *: Features and philosophy - Python is a multi-paradigm programming l
# *: Syntax and semantics - Python is meant to be an easily readable
# **: Indentation - Python uses whitespace indentation, rath
# **: Statements and control flow - Python's statements include (among other
# **: Expressions - Some Python expressions are similar to l
```
### 출처

좀더 디테일한 기능은 위키피디아api 페이지에서 확인 할 수 있다.  
[Wikipedia-AP](https://pypi.org/project/Wikipedia-API/)