# 'Better way 48. __init_subclass__를 사용해 하위 클래스를 검증하라' 정리


## 0. 들어가며

[Effective Python 2nd 파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)을 제대로 이해하고자 블로그에 정리합니다.

{{< figure src="http://image.kyobobook.co.kr/images/book/xlarge/191/x4801165213191.jpg" width="180px" >}}

<br/>
<br/>

**현재 위치**
{{< admonition note >}}
<6. Metaclasses and Attributes>  
Item 48: Validate Subclasses with `__init_subclass__`  
**Better way 48. `__init_subclass__`를 사용해 하위 클래스를 검증하라**
{{< /admonition >}}


<br/>

---

## 1. 한 줄 요약 및 첨언

하위 클래스가 제대로 구현됐는지 확인하기 위해서는 `__init_subclass__`를 사용합니다. (파이썬 3.6에서 추가됨)

메타클래스를 정의하는 방법이 있었지만, 요게 훨씬 낫습니다.


<br/>

---

## 2. 사용 예시

하위 클래스의 sides 가 3 이상인지 검증하는 예제입니다.

```python
# Example 4
class BetterPolygon:
    sides = None  # Must be specified by subclasses

    def __init_subclass__(cls):
        print(f'* Running __init_subclass__ for {cls}')
        super().__init_subclass__()
        if cls.sides < 3:
            raise ValueError('Polygons need 3+ sides')

    @classmethod
    def interior_angles(cls):
        return (cls.sides - 2) * 180


print('\n== Before BetterPolygon')
class Hexagon(BetterPolygon):
    sides = 6
print('Hexagon', Hexagon.interior_angles())  # Hexagon 720


print('\n== Before Point')
try:
    class Point(BetterPolygon):
        sides = 1

except ValueError as e:
    print('ValueError!!!', e)  # ValueError!!! Polygons need 3+ sides
```

```output.txt
== Before BetterPolygon
* Running __init_subclass__ for <class '__main__.Hexagon'>
Hexagon 720

== Before Point
* Running __init_subclass__ for <class '__main__.Point'>
ValueError!!! Polygons need 3+ sides
```

BetterPolygon을 상속한 클래스를 정의하면, `__init_subclass__` 가 불려서 클래스가 잘 정의되었는지 확인할 수 있습니다.

<br/>

---

## 3. 기억해야 할 내용

책에서 챕터 마지막 부분에 적혀있는 내용입니다.

{{< admonition tip >}}
The `__new__` method of metaclasses is run after the `class` statement's entire body has been processed.  
**메타클래스의 `__new__` 메서드는 class 문의 모든 본문이 처리된 직후에 호출된다.**
{{< /admonition >}}

{{< admonition tip >}}
Metaclasses can be used to inspect or modify a class after it's defined but before it's created, but they're often more heavyweight than what you need.  
**메타클래스를 사용해 클래스가 정의된 직후이면서 클래스가 생성되기 직전인 시점에 클래스 정의를 변경할 수 있다. 하지만 메타클래스는 원하는 목적을 달성하기에 너무 복잡해지는 경우가 많다.**
{{< /admonition >}}

{{< admonition tip >}}
Use `__init_subclass__` to ensure that subclasses are well formed at the time they are defined, before objects of their type are constructed.  
**`__init_subclass__`를 사용해 하위 클래스가 정의된 직후, 하위 클래스 타입이 만들어지기 직전에 해당 클래스가 원하는 요건을 잘 갖췄는지 확인하라.**
{{< /admonition >}}

{{< admonition tip >}}
Be sure to call `super().__init_subclass__` from within your class's `__init_subclass__` definition to enable validation in multiple layers of classes and multiple inheritance.  
**`__init_subclass__` 정의 안에서 `super().__init_subclass__`를 호출해 여러 계층에 걸쳐 클래시를 검증하고 다중 상속을 제대로 처리하도록 하라.**
{{< /admonition >}}

<br/>

---

## 4. 더 알아보기

### 4.1 메타클래스를 사용했다면

`__init_subclass__`를 사용할 거지만, 예전의 메타클래스 방식도 알아두도록 합시다.

```python
from pprint import pprint


# Example 2
class ValidatePolygon(type):  # 메타클래스 정의
    def __new__(meta, name, bases, class_dict):
        # Only validate subclasses of the Polygon class
        print(f'* Running {meta}.__new__ for {name}')
        print('Bases:', bases)
        pprint(class_dict)
        if bases:
            if class_dict['sides'] < 3:
                raise ValueError('Polygons need 3+ sides')
        return type.__new__(meta, name, bases, class_dict)


print('\n== Before Polygon')
class Polygon(metaclass=ValidatePolygon):
    sides = None  # Must be specified by subclasses

    @classmethod
    def interior_angles(cls):
        return (cls.sides - 2) * 180


print('\n== Before Triangle')
class Triangle(Polygon):
    sides = 3
print('Triangle', Triangle.interior_angles())


print('\n== Before Line')
try:
    class Line(Polygon):
        print('Before sides')
        sides = 2
        print('After sides')  # 여기도 불림
except ValueError as e:
    print('ValueError!!', e)
```

메타클래스를 상속받은 클래스의 정의 코드가 실행되면, 메타 클래스의 `__new__`가 실행되고,  
여기에서 클래스가 잘 구현되었는지 검증할 수 있습니다.

```output.txt
== Before Polygon
* Running <class '__main__.ValidatePolygon'>.__new__ for Polygon
Bases: ()
{'__module__': '__main__',
 '__qualname__': 'Polygon',
 'interior_angles': <classmethod object at 0x00000263CF7A91F0>,
 'sides': None}

== Before Triangle
* Running <class '__main__.ValidatePolygon'>.__new__ for Triangle
Bases: (<class '__main__.Polygon'>,)
{'__module__': '__main__', '__qualname__': 'Triangle', 'sides': 3}
Triangle 180

== Before Line
Before sides
After sides
* Running <class '__main__.ValidatePolygon'>.__new__ for Line
Bases: (<class '__main__.Polygon'>,)
{'__module__': '__main__', '__qualname__': 'Line', 'sides': 2}
ValueError!! Polygons need 3+ sides
```

`sides` 값이 2가 들어와서 ValueError 를 발생시켰습니다.

<br/>

---


## 10. 참고 자료

- [파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)
- [원저자 깃허브](https://github.com/bslatkin/effectivepython/blob/master/example_code/item_48.py)

<br/>

---
