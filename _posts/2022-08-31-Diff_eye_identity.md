---
published: true
title:  "[Numpy] eye 함수 와 identity 함수의 차이"
categories: Numpy
tag: [Numpy, Python, data, matrix]
---

Numpy 라이브러리의 eye 함수와 identity 함수는  
둘다 단위 행열을 만들어주는 동일한 기능을 가진 함수다.  
그런데 왜 두개로 나뉜걸까? 두 함수에 차이가 있는것은 아닐까?

## 함수 내부를 들여다 보자

identity 함수의 소스코드를 보면 바로 답이 나온다.
```py
    if like is not None:
        return _identity_with_like(n, dtype=dtype, like=like)

    from numpy import eye
    return eye(n, dtype=dtype, like=like) ## eye 함수를 호출한다.
```
내부적으로는 eye 함수를 호출하는 것을 볼 수 있다..  
결국 똑같은 기능을 하는 함수라는 것!  


## eye 함수의 파라미터
사실 eye 함수에서만 사용할 수 있는 파라미터가 몇가지 더 있다.  

k : int, optional  
대각선의 위치를 바꾼다.
```py
print(np.eye(3,k=1))
print(np.eye(3,k=-1))

# print
[[0. 1. 0.]
 [0. 0. 1.]
 [0. 0. 0.]]
[[0. 0. 0.]
 [1. 0. 0.]
 [0. 1. 0.]]
```
order : {'C', 'F'}, optional  
포트란 호환을 위한 파라미터 같은데  
메모리에 적재되는 순서가 row-major 인지 column-major 인지 선택 할 수 있다.  
'C' 나 'F'중 하나를 선택 할 수 있으며  
C는 C-style로 row-major, F는 Fortran-style로 column-major이다.

![row-major_column-major](/images/2022-08-31-Diff_eye_identity_0.png)

