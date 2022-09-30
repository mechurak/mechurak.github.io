---
title: "'Better way 17. 내부 상태에서 원소가 없는 경우를 처리할 때는 setdefault보다 defaultdict를 사용하라' 정리"
date: 2020-12-11T01:00:00+09:00
summary: "defaultdict 을 사용하면, 키가 없는 경우를 편리하게 처리할 수 있습니다."
categories: [ python ]
series: [ effective python 2. 리스트와 딕셔너리 ]
series_weight: 17
# featuredImage: http://image.kyobobook.co.kr/images/book/xlarge/191/x4801165213191.jpg
tags:
- python
---

## 들어가며

[Effective Python 2nd 파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)을 제대로 이해하고자 블로그에 정리합니다.

{{< figure src="http://image.kyobobook.co.kr/images/book/xlarge/191/x4801165213191.jpg" width="180px" >}}

<br/>
<br/>

**현재 위치**
{{< admonition note >}}
<2. Lists and Dictionaries>  
Item 17: Prefer defaultdict Over setdefault to Handle Missing Items in Internal State  
**Better Way 17. 내부 상태에서 원소가 없는 경우를 처리할 때는 setdefault보다 defaultdict를 사용하라**
{{< /admonition >}}


<br/>

---


## 한 줄 요약 및 첨언

defaultdict 을 사용하면, 키가 없는 경우를 편리하게 처리할 수 있습니다.

보통은 `dict`를 `get()` 을 통해 조회하고,  
조금 복잡한 상황이라면 `dict` 대신에 `defaultdict` 을 사용하는 걸 고려해 보면 좋을 것 같습니다.

<br/>

---

## 사용 예시

```python
from collections import defaultdict


class Visits:
    def __init__(self):
        # 없는 키를 조회하면 set() (빈 set)을 해당 키에 연관시키고 리턴해 줌
        self.data = defaultdict(set) 

    def add(self, country, city):
        self.data[country].add(city)


visits = Visits()
visits.add('영국', '바스')
visits.add('영국', '런던')
print(visits.data)
# defaultdict(<class 'set'>, {'영국': {'런던', '바스'}})
```


<br/>

---

## 기억해야 할 내용

책에서 챕터 마지막 부분에 적혀있는 내용입니다.

{{< admonition tip >}}
If you're creating a dictionary to manage an arbitrary set of potential keys, then you should prefer using a `defaultdict` instance from the `collections` built-in module if it suits your problem.  
**키로 어떤 값이 들어올지 모리는 딕셔너리를 관래해야 하는데 collections 내장 모둘에 있는 defaultdict 인스턴스가 여러분의 필요에 맞아 떨어진다면 defaultdict를 사용하라.**
{{< /admonition >}}

{{< admonition tip >}}
If a dictionary of arbitrary keys is passed to you, and you don't control its creation, then you should prefer the `get` method to access its items. However, it's worth considering using the `setdefault` method for the few situations in which it leads to shorter code.  
**임의의 키가 들어있는 딕셔너리가 여러분에게 전달됐고 그 딕셔너리가 어떻게 생성됐는지 모르는 경우, 딕셔너리의 원소에 접근하려면 우선 get을 사용해야 한다. 하지만 setdefault가 더 짧은 코드를 만들어내는 몇 가지 경우에는 setdefault를 사용하는 것도 고려해볼 만하다.**
{{< /admonition >}}

<br/>

---

## 상세 설명

위 Visits 클래스의 add 에서 setdefault 를 사용한다면

```python
class Visits:
    def __init__(self):
        self.data = {}

    def add(self, country, city):
        city_set = self.data.setdefault(country, set())
        city_set.add(city)
```

`setdefault`라는 메서드 이름이 좀 애매해서 코드를 처음 읽는 사람이 동작을 바로 이애하기 어렵습니다.  
그리고 `setdefault()` 부를 때, 키가 있더라도 `set()`이 항상 불리므로 효율적이라 할 수 없습니다.

<br/>

---


## 참고 자료

- [파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)
- [길벗출판사 깃허브](https://github.com/gilbutITbook/080235/blob/master/Chapter2/Better%20way17.py)

<br/>

---