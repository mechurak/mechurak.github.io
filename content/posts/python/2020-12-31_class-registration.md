---
title: "'Better way 49. __init_subclass__를 사용해 클래스 확장을 등록하라' 정리"
date: 2020-12-29T01:00:00+09:00
summary: "__init_subclass__를 통해서 하위 클래스 정의하면 각 클래스마다 꼭 불려야 하는 로직을 자동으로 수행할 수 있습니다."
categories: [ python ]
series: [ effective python 6. 메타클래스와 애트리뷰트 ]
series_weight: 49
# featuredImage: http://image.kyobobook.co.kr/images/book/xlarge/191/x4801165213191.jpg
tags:
- python
---

## 0. 들어가며

[Effective Python 2nd 파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)을 제대로 이해하고자 블로그에 정리합니다.

{{< figure src="http://image.kyobobook.co.kr/images/book/xlarge/191/x4801165213191.jpg" width="180px" >}}

<br/>
<br/>

**현재 위치**
{{< admonition note >}}
<6. Metaclasses and Attributes>
Item 49: Register Class Existence with `__init_subclass__`
**Better way 49. `__init_subclass__`를 사용해 클래스 확장을 등록하라**
{{< /admonition >}}


<br/>

---

## 1. 한 줄 요약 및 첨언

`__init_subclass__` 통해서 하위 클래스 정의하면 각 클래스마다 꼭 불려야 하는 로직을 자동으로 수행할 수 있습니다.


<br/>

---

## 2. 사용 예시

객체를 json string 으로 바꾸기도 하고, 해당 string으로부터 다시 객체로 만드는 기능이 필요한 상황입니다.
​
클래스 등록 부분입니다. registry 변수에 하위 클래스들의 이름을 등록하고, deserialize때 사용하려는 목적입니다.
​
```python
import json
​
# Example 6
registry = {}  # 클래스 이름 저장용 딕셔너리
​
​
# 클래스 이름 등록하는 함수. 클래스 정의때 요게 불려야 함
def register_class(target_class):
    registry[target_class.__name__] = target_class
​
​
# json str 으로부터 다시 객체 만들어주는 함수
def deserialize(data: str):
    params = json.loads(data)
    name = params['class']
    target_class = registry[name]
    return target_class(*params['args'])  # *params['args']: 리스트의 원소들을 arguments 로 넘김
```
​

<br>

베이스가 되는 클래스입니다. `__init_subclass__` 를 구현해서, `register_class()`가 자동으로 호출이 됩니다. (수동으로 호출할 필요가 없어 졌습니다.)
​
```python
# Example 5 + Example 13
class BetterSerializable:  # __init_class__ 구현한 베이스 클래스
    def __init__(self, *args):
        self.args = args
​
    def serialize(self):
        return json.dumps({
            'class': self.__class__.__name__,
            'args': self.args,
        })
​
    def __repr__(self):
        name = self.__class__.__name__
        args_str = ', '.join(str(x) for x in self.args)
        return f'{name}({args_str})'
​
    def __init_subclass__(cls):  # 요 메서드 구현
        super().__init_subclass__()
        register_class(cls)  # 하위 클래스가 정의 될 때, 요게 자동으로 불리도록 하기 위함.
```

<br>
​
위 클래스를 상속받은 하위클래스와 사용 예제입니다.

```python
# Example 7
class EvenBetterPoint2D(BetterSerializable):
    def __init__(self, x, y):
        super().__init__(x, y)
        self.x = x
        self.y = y
​
# Example 8
before = EvenBetterPoint2D(5, 3)
print('Before:    ', before, type(before))  # Before:     EvenBetterPoint2D(5, 3) <class '__main__.EvenBetterPoint2D'>
data = before.serialize()
print('Serialized:', data, type(data))  # Serialized: {"class": "EvenBetterPoint2D", "args": [5, 3]} <class 'str'>
after = deserialize(data)
print('After:     ', after, type(after))  # After:      EvenBetterPoint2D(5, 3) <class '__main__.EvenBetterPoint2D'>
```

<br/>

---

## 3. 기억해야 할 내용

책에서 챕터 마지막 부분에 적혀있는 내용입니다.

{{< admonition tip >}}
Class registration is a helpful pattern for building modular Python programs.
**클래스 등록은 파이썬 프로그램을 모듈화할 때 유용한 패턴이다.**
{{< /admonition >}}

{{< admonition tip >}}
Metaclasses let you run registration code automatically each time a base class is subclassed in a program.
**메타클래스를 사용하면, 프로그램 안에서 기반 클래스를 상속한 하위 클래스가 정의될 때마다 등록 코드를 자동으로 실행할 수 있다.**
{{< /admonition >}}

{{< admonition tip >}}
Using metaclasses for class registration helps you avoid errors by ensuring that you never miss a registration call.
**메타클래스를 클래스 등록에 사용하면 클래스 등록 함수를 호출하지 않아서 생기는 오류를 피할 수 있다.**
{{< /admonition >}}

{{< admonition tip >}}
Prefer `__init_subclass__` over standard metaclass machinery because it's clearer and easier for beginners to understand.
**표준적인 메타클래스 방식보다는 `__init_sublcass__`가 더 낫다. `__init_subclass__` 쪽이 더 깔끔하고 초보자가 이해하기도 더 쉽다.**
{{< /admonition >}}

<br/>

---

## 4. 더 알아보기

### 4.1 메타클래스를 사용했다면

메타클래스를 사용한 방식도 알아두기는 합시다.

```python
# 클래스 등록 부분은 같은 코드 사용

# Example 11
class Meta(type):
    def __new__(meta, name, bases, class_dict):
        cls = type.__new__(meta, name, bases, class_dict)
        register_class(cls)  # 하위 클래스가 정의 될 때, 요게 자동으로 불리도록 하기 위함.
        return cls


class TempSerializable(metaclass=Meta):  # metaclass 지정
    def __init__(self, *args):
        self.args = args

    def serialize(self):
        return json.dumps({
            'class': self.__class__.__name__,
            'args': self.args,
        })

    def __repr__(self):
        name = self.__class__.__name__
        args_str = ', '.join(str(x) for x in self.args)
        return f'{name}({args_str})'


# Example 12
class Vector3D(TempSerializable):
    def __init__(self, x, y, z):
        super().__init__(x, y, z)
        self.x, self.y, self.z = x, y, z


before = Vector3D(10, -7, 3)
print('Before:    ', before)  # Before:     Vector3D(10, -7, 3)
data = before.serialize()
print('Serialized:', data)  # Serialized: {"class": "Vector3D", "args": [10, -7, 3]}
print('After:     ', deserialize(data))  # After:      Vector3D(10, -7, 3)
```

<br/>

---


## 10. 참고 자료

- [파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)
- [원저자 깃허브](https://github.com/bslatkin/effectivepython/blob/master/example_code/item_49.py)

<br/>

---