---
title: "'Better Way 18. __missing__을 사용해 키에 따라 다른 디폴트 값을 생성하는 방법을 알아두라' 정리"
date: 2020-12-11T01:01:00+09:00
summary: "dict 을 확장하고 __missing__을 구현하면, key에 따라 다른 디폴트 값을 돌려 주게 할 수 있습니다."
categories: [ python ]
series: [ effective python 2. 리스트와 딕셔너리 ]
series_weight: 18
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
Item 18: Know How to Construct Key-Dependent Default Values with `__missing__`  
**Better Way 18. `__missing__`을 사용해 키에 따라 다른 디폴트 값을 생성하는 방법을 알아두라**
{{< /admonition >}}


<br/>

---


## 한 줄 요약 및 첨언

키가 없는 상황을 처리하고자 할 때, `defaultdict` 으로도 해결이 안되는 상황이 올 수도 있습니다.  
(defaultdict 은 키에 따라 다른 동작을 하게 할 수 없습니다.)  
이 때, `dict` 을 확장하고 `__missing__()` 을 구현하면, key에 따라 다른 디폴트 값을 돌려 주게 할 수 있습니다.

<br/>

---

## 사용 예시

```python
def open_picture(profile_path):
    try:
        return open(profile_path, 'a+b')
    except OSError:
        print(f'경로를 열 수 없습니다: {profile_path}')
        raise


class Pictures(dict):
    def __missing__(self, key):
        value = open_picture(key)
        self[key] = value
        return value

pictures = Pictures()
handle = pictures[path]
handle.seek(0)
image_data = handle.read()
```


<br/>

---

## 기억해야 할 내용

책에서 챕터 마지막 부분에 적혀있는 내용입니다.

{{< admonition tip >}}
The `setdefault` method of `dict` is a bad fit when creating the default value has high computational cost or may raise exceptions.  
**디폴트 값을 만드는 계산 비용이 높거나 만드는 과정에서 예외가 발생할 수 있는 상황에서는 `dict`의 `setdefault` 메서드를 사용하지 말라.**
{{< /admonition >}}

{{< admonition tip >}}
The function passed to `defaultdict` must not require any arguments, which makes it impossible to have the default value depend on the key being accessed.  
**`defaultdict`에 전달되는 함수는 인자를 받지 않는다. 따라서 접근에 사용한 키 값에 맞는디폴트 값을 생성하는 것은 불가능하다.**
{{< /admonition >}}

{{< admonition tip >}}
You can define your own `dict` subclass with a `__missing__` method in order to construct default values that must know which key was being accessed.  
**디폴트 키를 만들 때 어떤 키를 사용했는지 반드시 알아야 하는 상황이라면 직접 dict의 하위 클래스와 `__mising__` 메서드를 정의하면 된다.**
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
- [길벗출판사 깃허브](https://github.com/gilbutITbook/080235/blob/master/Chapter2/Better%20way18.py)

<br/>

---