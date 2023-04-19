---
published: true
title:  "[matplotlib, python] figsize 조정이 안될 때"
categories: [matplotlib, python]
tag: [matplotlib, pandas, Numpy, python, data]
---

matplotlib에서 그래프의 사이즈를 키우려면 'figure' 함수를 사용한다.

그런데 함수가 아래쪽에 있으면 동작하지 않는다.
 
```py
plt.hist(sample_of_10, alpha=.5);
plt.hist(sample_of_100, alpha=.5);
plt.axvline(x=pop_ratio, c='red', label = 'Population Mean');
plt.figure(figsize=(8,5)); # NOT WORKING
plt.legend();
```
아래처럼 그래프를 그리기 전에 먼저 설정을 해줘야 동작한다.

```py
plt.figure(figsize=(8,5)); # WORKING
plt.hist(sample_of_10, alpha=.5);
plt.hist(sample_of_100, alpha=.5);
plt.axvline(x=pop_ratio, c='red', label = 'Population Mean');
plt.legend();
```

처음 matplotlib을 사용할 때 이걸 몰라서  왜 안되는지 한참을 고민했다.  
나같은 사람 또 있으면 도움이 되길 바란다.