---
published: true
title:  "[Pandas] 파이썬 판다스 원하는 행/열 가져오기 총정리"
categories: DATA
tag: [Data, pandas, numpy, python, loc, iloc]
---
<br>  
  
pandas.DataFrame 에는 원하는 행과 열의 데이터를  
가져올수 있는 방법이 여러 가지가 있는데  
처음 라이브러리를 사용하니 방법이 너무 다양하고  
방법마다 가져오는 데이터 타입이 달라서 꽤 헷갈렸다.  
그래서 복습 겸 내가 볼 겸 정리를 해보려고 한다.  

<br>  

아래 블로그의 내용을 요약해서 작성했다.
> https://jimmy-ai.tistory.com/226

<br>

### 데이터 테이블  
<br>

```py
import pandas as pd

a = {   '열0' : [1, 2, 3, 4, 5], 
        '열1' : [10, 20, 30, 40, 50], 
        '열2' : [100, 200, 300, 400, 500],
        '열3' : [1000, 2000, 3000, 4000, 5000]};
df = pd.DataFrame(a, index = ['행0', '행1', '행2', '행3', '행4']);
```
|index|DATA|DATA|DATA|DATA|
|:--:|:--:|:--:|:--:|:--:|
|---|열0|열1|열2|열3|
|행0|1|10|100|1000|
|행1|2|20|200|2000|
|행2|3|30|300|3000|
|행3|4|40|400|4000|
|행4|5|50|500|5000|


---
### 열 추출
<br>  

대괄호에 해당 column의 이름을 넣으면 Series 형식으로 열을 추출한다.
```py
df = df['열0'];
print(df);
print(type(df));

# 결과
행0    1
행1    2
행2    3
행3    4
행4    5
Name: 열0, dtype: int64
<class 'pandas.core.series.Series'>
```
<br>  
  

대괄호 2쌍으로에 해당 column의 이름을 넣으면 DataFrame 형식으로 column을 추출.
```py
df = df[['열0']];
print(df);
print(type(df));

# 결과
    열0
행0   1
행1   2
행2   3
행3   4
행4   5
<class 'pandas.core.frame.DataFrame'>
```
<br>  
  
  
여러개의 column을 가져올 때는 대괄호 2개를 사용
```py
df = df[['열0','열1']];
print(df);
print(type(df));

# 결과
    열0  열1
행0   1  10
행1   2  20
행2   3  30
행3   4  40
행4   5  50
<class 'pandas.core.frame.DataFrame'>
```
```py
파이썬에서 자료들을 대괄호로 묶는 것은 리스트 형식이 된다.  
data = ['A','B'];
print(type(data)); # <class 'list'>

결과적으로 df[]는 리스트를 인자값을 받을 수 있고

print(df[['A','B']]);
print(df[data]);

두가지의 결과는 같다.
```
---
### 행 추출
<br>  

기본 개념은 열과 같고 loc 메소드를 사용한다는 점만 다르다.  

loc 뒤의 대괄호에 해당 row의 이름을 넣으면 Series 형식으로 row를 추출한다.
```py
df = df.loc['행0'];
print(df);
print(type(df));

# 결과
열0       1
열1      10
열2     100
열3    1000
Name: 행0, dtype: int64
<class 'pandas.core.series.Series'>
```
대괄호 2쌍으로에 해당 row의 이름을 넣으면 DataFrame 형식으로 row를 추출.
```py
df = df.loc[['행0']];
print(df);
print(type(df));

# 결과
    열0  열1   열2    열3
행0   1  10  100  1000
<class 'pandas.core.frame.DataFrame'>
```
여러개의 row를 가져올 때는 대괄호 2개를 사용
```py
df = df.loc[['행0','행1']];
print(df);
print(type(df));

# 결과
    열0  열1   열2    열3
행0   1  10  100  1000
행1   2  20  200  2000
<class 'pandas.core.frame.DataFrame'>
```
0부터 시작하는 숫자 index를 가진경우 
```py
df.loc[0:5] # 인덱스 0부터 4까지의 행을 가져온다.
```
이런 식으로 작성하면 DataFrame 형식으로 가져온다.

---
### 행과 열을 모두 지정 추출
<br>

loc의 두번째 인자로 열이름을 리스트로 넣으면 DataFrame 으로 추출
```py
df = df.loc[['행0','행1','행3'], ['열0','열1']];
print(df);
print(type(df));

# 결과
    열0  열1
행0   1  10
행1   2  20
행3   4  40
<class 'pandas.core.frame.DataFrame'>
```

: 를 사용해 전체 행을 가져올 수 있다.
```py
df = df.loc[:, ['열0','열1']];
print(df);
print(type(df));

# 결과
    열0  열1
행0   1  10
행1   2  20
행2   3  30
행3   4  40
행4   5  50
<class 'pandas.core.frame.DataFrame'>
```
---
### 조건 인덱싱
<br>  

Column A 의 값이 10 초과인 경우만 모아서 DataFrame으로 추출
```py
df = df.loc[df['열1'] > 20];
print(df);
print(type(df));

# 결과
    열0  열1   열2    열3
행2   3  30  300  3000
행3   4  40  400  4000
행4   5  50  500  5000
<class 'pandas.core.frame.DataFrame'>
```

두가지 이상의 조건도 인덱싱가능
```py
df = df.loc[(df['열0'].isin([1,2,3])) & (df['열1'] > 10)];
print(df);
print(type(df));

# 결과
    열0  열1   열2    열3
행1   2  20  200  2000
행2   3  30  300  3000
<class 'pandas.core.frame.DataFrame'>
```

---
### 행과 열의 위치를 기준으로 인덱싱
<br>  

1 - 3 행까지 추출
```py
df = df.iloc[1:4];
print(df);
print(type(df));

# 결과
    열0  열1   열2    열3
행1   2  20  200  2000
행2   3  30  300  3000
행3   4  40  400  4000
<class 'pandas.core.frame.DataFrame'>
```

행 0 - 1, 열 1 부터 끝까지 추출
```py
df = df.iloc[0:2, 1:];
print(df);
print(type(df));

# 결과
    열0  열1   열2    열3
행1   2  20  200  2000
행2   3  30  300  3000
행3   4  40  400  4000
<class 'pandas.core.frame.DataFrame'>
```

---
### 단일 데이터 인덱싱
<br>  

단일 데이터 인덱싱에는 loc 와 at 둘다 사용이 가능하다.  
loc 의 경우 범위 지정이 가능하지만  
at 의 경우 딱 1개의 데이터만 접근이 가능하다.

하지만 실행속도는 at이 더 빠르다.

```py
df.loc[100, 'pclass'] # pclass 열의 100번 인덱스를 가지는 행의 값
df.at[100, 'pclass'] # pclass 열의 100번 인덱스를 가지는 행의 값

df.loc[100:102, 'pclass'] # 정상작동 Series 반환
df.at[100:102, 'pclass'] # ValueError

df.loc[100, ['pclass','age']] # 정상작동
df.at[100, ['pclass','age']] # TypeError

df.loc[100:102, ['pclass','age']] # 정상작동
df.loc[100:102, ['pclass','age']] # ValueError
```