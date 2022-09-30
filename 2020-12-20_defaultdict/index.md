# 디폴트 값을 자동으로 만들어 주는 defaultdict 컨테이너



## 0. 들어가며

파이썬을 사용할 때 리스트와 딕셔너리는 정말 큰 비중을 차지하는 것 같습니다.  
그중에서 딕셔너리는 특정 키가 없는 상황을 잘 처리해야 합니다.  
이 때, 유용한 `defaultdict` 객체에 대해서 정리하려고 합니다.

기존 리스트에서 약간의 변형이 필요하거나 필터링이 필요할 때, 리스트 컴프리헨션을 이용해 짧은 코드로 원하는 리스트를 손쉽게 만들 수 있습니다.

<br/>

{{< figure src="/images/python.jpg" width="300px" >}}

딸내미가 그려준 파이썬 로고를 자랑하고 싶었던 건 안비밀입니다.


<br/>

---

## 1. 한 줄 요약

`defaultdict`은 `dict` 대신에 사용할 수 있습니다. 없는 키로 접근하면 해당 키에 디폴트 값을 일단 할당한 후 리턴해 줍니다.  
없는 키에 대한 부담없이 딕셔너리를 사용할 수 있게 됩니다.


<br/>

---


## 2. 사용방법

```python
from collections import defaultdict

s = 'mississippi'
d = defaultdict(int)  # 없는 키로 접근하면, 먼저 해당 키에 `int()` 호출해서 대입해 줌
for k in s:
    d[k] += 1  # d[k] 에 적절한 값이 항상 있다고 확신할 수 있음

temp = sorted(d.items())
print(temp)  # [('i', 4), ('m', 1), ('p', 2), ('s', 4)]

print(d)  # defaultdict(<class 'int'>, {'m': 1, 'i': 4, 's': 4, 'p': 2})
```


<br/>

---


## 3. 더 알아보기

### 3.1 그냥 dict을 사용 (get)

없는 키에 대해서 리턴 받을 값을 지정할 수 있는 `dict.get()`[^1]을 사용하면 아래처럼 됩니다.

[^1]: `dict.get(key [,default])`
    [파이썬 공식 문서 링크](http://docs.python.org/3/library/stdtypes.html#dict.get)
    딕셔너리에 key가 있으면 해당 value 리턴.
    없으면 인자로 받은 default 값(안 넘겼으면 None)을 리턴.


```python
s = 'mississippi'
d = {}
for k in s:
    d[k] = d.get(k, 0) + 1

temp = sorted(d.items())
print(temp)  # [('i', 4), ('m', 1), ('p', 2), ('s', 4)]

print(d)  # {'m': 1, 'i': 4, 's': 4, 'p': 2}
```

<br/>

### 3.2 그냥 dict을 사용 (setdefault)

`dict.setdefault()`[^2]을 사용할 수도 있지만, `setdefault`는 이름이 좀 애매해서(혼동을 줄 수 있어서) 비추하는 것 같습니다.

[^2]: `dict.setdefault(key [,default])`
    [파이썬 공식 문서 링크](http://docs.python.org/3/library/stdtypes.html#dict.setdefault)
    딕셔너리에 key가 있으면 해당 value 리턴.
    없으면 인자로 받은 default 값(안 넘겼으면 None)을 해당 키에 할당하고 리턴.


```python
s = 'mississippi'
d = {}
for k in s:
    d[k] = d.setdefault(k, 0) + 1

temp = sorted(d.items())
print(temp)  # [('i', 4), ('m', 1), ('p', 2), ('s', 4)]

print(d) # {'m': 1, 'i': 4, 's': 4, 'p': 2}
```


<br/>

---


## 10. 참고자료

- [defaultdict objects / Python 공식문서](https://docs.python.org/3/library/collections.html?highlight=defaultdict#defaultdict-objects)
- ['Better way 17. 내부 상태에서 원소가 없는 경우를 처리할 때는 setdefault보다 defaultdict를 사용하라' 정리](https://mechurak.github.io/ko/posts/python/2020-12-11_default-dict/)

<br/>

