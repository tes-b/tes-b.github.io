---
published: true
title:  "[python] itertools - product, permutations, combinations"
categories: python
tag: [python, itertools, product, permutations, combinations]
---

itertools의 함수들로 순열 조합을 구현하는 방법.
<br>

---
## product 
원소들의 데카르트곱을 구해준다.  
모든 원소가 모든 포지션에 한번씩 들어간다.

```py
import itertools

A = [1,2,3]
res = list(itertools.product(A,repeat=2));

print(res);

# print
[(1, 1), (1, 2), (1, 3), (2, 1), (2, 2), (2, 3), (3, 1), (3, 2), (3, 3)]
```
```py
A = [1,2];
res = list(itertools.product(A,repeat=3));

for ele in res:
    print(ele);

# print
(1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 2, 2)
(2, 1, 1)
(2, 1, 2)
(2, 2, 1)
(2, 2, 2)
```
```py
B = [[1,2,3], [4,5]]
res = list(itertools.product(*B)) 

for ele in res:
    print(ele);

# print
(1, 4)
(1, 5)
(2, 4)
(2, 5)
(3, 4)
(3, 5)
```
```py
B = [[1,2,3], [4,5]]
res = list(itertools.product(B)) 

for ele in res:
    print(ele);

# print
([1, 2, 3],)
([4, 5],)
```
```py
C = [1,2,3];
D = [4,5];
res = list(itertools.product(C,D));

for ele in res:
    print(ele);

#print
(1, 4)
(1, 5)
(2, 4)
(2, 5)
(3, 4)
(3, 5)
```
<br>

## permutations
순열을 반환한다.  

```py
# permutations
E = [1,2];
res_pro = list(itertools.product(E,repeat=2));
res_per = list(itertools.permutations(E));

for ele in res_pro:
    print(ele);
    
print("\n");    
for ele in res_per:
    print(ele);

# print
(1, 1)
(1, 2)
(2, 1)
(2, 2)


(1, 2)
(2, 1)
```
```py
E = [1,2,3];
res_per = list(itertools.permutations(E));

for ele in res_per:
    print(ele);

# print
(1, 2, 3)
(1, 3, 2)
(2, 1, 3)
(2, 3, 1)
(3, 1, 2)
(3, 2, 1)
```
<br>

## combinations
위치에 상관없이 가능한 원소 조합만을 반환한다.
```py
F = [1,2,3];
res_com_2 = list(itertools.combinations(F,2));
res_com_3 = list(itertools.combinations(F,3));

print(res_com_2);
print(res_com_3);

# print
[(1, 2), (1, 3), (2, 3)] # res_com_2
[(1, 2, 3)]              # res_com_3
```
```py
G = [1,2,3,4,5];
res_com_4 = list(itertools.combinations(G,4));

for ele in res_com_4:
    print(ele);

# print
(1, 2, 3, 4)
(1, 2, 3, 5)
(1, 2, 4, 5)
(1, 3, 4, 5)
(2, 3, 4, 5)
```
<br>

## combination_with_replacement
combinations 와 같지만 원소를 중복 사용한 조합을 반환한다. 
```py
H = [1,2,3];
res_cwr = list(itertools.combinations_with_replacement(H,2));

for ele in res_cwr:
    print(ele);

# print
(1, 1)
(1, 2)
(1, 3)
(2, 2)
(2, 3)
(3, 3)
```