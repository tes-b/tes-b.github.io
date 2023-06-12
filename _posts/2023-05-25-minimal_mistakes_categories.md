---
published: True
title:  "깃허브 블로그 카테고리 만들기 초간단 방법 (minimal mistakes)"
categories: [Etc]
tag: [Etc, minimal-mistakes]
---

{% raw %}
깃허브 블로그를 운영하는 사람중 꽤 많은 사람이  
minimal mistakes 테마를 사용하는 것으로 보인다.  

사이드바에 카테고리를 표시하는 방법을 알려주는 블로그도 많은데  
예쁘게 나오기는 하지만 다소 귀찮은 방법들도 있다.

그래서 예쁘진 않더라도 간단하게 사이드바에 카테고리목록을 표시하는 방법을 소개한다.  

## 카테고리 추가

```_includes``` 폴더 안의 ```author-profile.html``` 파일로 간다.  

```sidebar.html```에 추가하지 않는 이유는  
메인 페이지에 사이드바를 추가하지 않았다면  
프로필만 표시되고 카테고리 목록이 안보이기 때문이다.  

아무튼 ```author-profile.html```파일의 맨 아래로 가면   

```html
{% include author-profile-custom-links.html %}
```
위와 같이 커스텀 프로파일 링크를 추가하는 코드가 있는데  

바로 아래에 다음 코드를 추가하고 문제없이 잘 나오는지 확인하면 된다.  

```html
<!-- Categories -->
<nav class="nav__list">
<ul class="nav__items" id="category_tag_menu">
    <li>
        <span class="nav__sub-title">📂 Total ({{site.posts | size}})</span>
        <ul>
            {% for category in site.categories %}
                {% if category %}
                <li>
                    <a href="/categories/#{{category[0] | slugify}}" class="">{{category[0]}} ({{category[1].size}})</a>
                </li>
                {% endif %}
            {% endfor %}
        </ul>
    </li>
</ul>
</nav>
```

## 코드 설명

```html
<span class="nav__sub-title">📂 Total ({{site.posts | size}})</span>
```
전체 포스트 수를 표시하는 코드


```html
{% for category in site.categories %}
    {% if category %}
        <li>
            <a href="/categories/#{{category[0] | slugify}}" class="">{{category[0]}} ({{category[1].size}})</a>
        </li>
    {% endif %}
{% endfor %}
```
각 카테고리 제목과 게시글 수를 표시하고 카테고리 목록으로 가는 링크를 만드는 코드

{% endraw %}