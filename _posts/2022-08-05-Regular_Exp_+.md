---
published: true
title:  "정규표현식에서 '+' 사용 예시"
categories: DATA
tag: [Data, pandas, numpy, python, '정규표현식']
---

### 정규표현식 '+' 사용 예시  
<br>


```py
str_test = pd.Series(['11112222^^^^',
                      '^^^^112233^^^^']);
```
```py
str_test_0 = str_test.str.replace(r'1','@');

# print(str_test_0);
0      @@@@2222^^^^
1    ^^^^@@2233^^^^
```
```py
str_test_1 = str_test.str.replace(r'1+','@');

# print(str_test_1);
0        @2222^^^^
1    ^^^^@2233^^^^
```
```py
str_test_2 = str_test.str.replace(r'[1-2]','@');

# print(str_test_2);
0      @@@@@@@@^^^^
1    ^^^^@@@@33^^^^
```
```py
str_test_3 = str_test.str.replace(r'[1-2]+','@');

# print(str_test_3);
0          @^^^^
1    ^^^^@33^^^^
```

추가 { } 사용법

```py
str_test_4 = str_test.str.replace(r'1{2}','@');

# print(str_test_4);
0       @@2222^^^^
1    ^^^^@2233^^^^
```
```py
str_test_5 = str_test.str.replace(r'[1-2]{2}','@');

# print(str_test_5);
0        @@@@^^^^
1    ^^^^@@33^^^^
```
<br>



3줄요약

>'+'를 붙일 경우 하나이상의 연속된 문자(1111)를 대체할 문자(@) 하나로 대체  
1111 -> @

>'+'를 안붙일경우 하나씩 대체  
1111 -> @@@@

>{n} 일경우 n개씩 대체  
