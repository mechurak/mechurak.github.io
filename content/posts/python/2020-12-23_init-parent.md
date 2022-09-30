---
title: "'Better way 40. super로 부모 클래스를 초기화하라' 정리"
date: 2020-12-23T01:00:00+09:00
summary: "부모 클래스의 초기화는 `super().__init__()` 으로 합니다."
categories: [ python ]
series: [ effective python 5. 클래스와 인터페이스 ]
series_weight: 40
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
<5. Classes and Interfaces>  
Item 40: Initialize Parent Classes with super  
**Better way 40. super로 부모 클래스를 초기화하라**
{{< /admonition >}}


<br/>

---

## 1. 한 줄 요약 및 첨언

부모 클래스의 초기화는 `super().__init__()` 으로 합니다.



<br/>

---

## 2. 사용 예시

```python
class MyBaseClass:
    def __init__(self, value):
        self.value = value


# Example 13
class ImplicitTrisect(MyBaseClass):
    def __init__(self, value):
        super().__init__(value)  # 부모 클래스 초기화
        self.value /= 3


print(ImplicitTrisect(9).value)  # 3.0
```

<br/>

---

## 3. 기억해야 할 내용

책에서 챕터 마지막 부분에 적혀있는 내용입니다.

{{< admonition tip >}}
Python's standard method resolution order (MRO) solves the problems of superclass initialization order and diamond inheritance.  
**파이썬은 표준 메서드 결정 순서(MRO)를 활용해 상위 클래스 초기화 순서와 다이아몬드 상속 문제를 해결한다.**
{{< /admonition >}}

{{< admonition tip >}}
Use the `super` built-in function with zero arguments to initialize parent classes.  
**부모 클래스를 초기화할 때는 super 내장 함수를 아무 인자 없이 호출하라. super를 아무 인자 없이 호출하면 파이썬 컴파일러가 자동으로 올바른 파라미터를 넣어준다.**
{{< /admonition >}}


<br/>

---

## 4. 더 알아보기

### 4.1 super 에 파라미터 전달

컴파일러가 알아서 넣어주니 전달할 필요 없습니다.  
첫 번째 파라미터는 접근하고 싶은 MRO 뷰를 제공할 부모 타입이고, 두 번째 파라미터는 지정한 MRO 뷰를 접근할 때 사용할 인스턴스라는데 그냥 계속 모르도록 합시다. ㅠㅜ

```python
class MyBaseClass:
    def __init__(self, value):
        self.value = value


# Example 12
class ExplicitTrisect(MyBaseClass):
    def __init__(self, value):
        super(ExplicitTrisect, self).__init__(value)
        self.value /= 3


# Example 13
class AutomaticTrisect(MyBaseClass):
    def __init__(self, value):
        super(__class__, self).__init__(value)
        self.value /= 3


class ImplicitTrisect(MyBaseClass):
    def __init__(self, value):
        super().__init__(value)
        self.value /= 3

print(ExplicitTrisect(9).value)  # 3.0
print(AutomaticTrisect(9).value)  # 3.0
print(ImplicitTrisect(9).value)  # 3.0
```

<br/>

### 4.2 다이아몬드 상속 문제

어떤 클래스가 두 가지 서로 다른 클래스를 상속하는데, 두 상위 클래스의 상위 계층을 올라가면 같은 조상 클래스가 존재하는 경우를 뜻합니다. 이런 경우에 공통 조상 클랫의 `__init__()`메서드가 여러 번 호출될 수 있습니다.  
이런 케이스도 super() 를 통해 초기화 하면 문제가 없습니다. super()를 사용하면 다이아몬드 계층의 공통 상위 클래스를 단 한번만 호출하도록 보장한다고 합니다. 내부적으로 표준 메서드 결정 순서(Method Resolution Order, MRO)가 사용된다고 합니다.

```python
class MyBaseClass:
    def __init__(self, value):
        print(f'{__class__}.__init__()')
        self.value = value


class TimesSevenCorrect(MyBaseClass):
    def __init__(self, value):
        print(f'{__class__}.__init__()')
        super().__init__(value)
        self.value *= 7


class PlusNineCorrect(MyBaseClass):
    def __init__(self, value):
        print(f'{__class__}.__init__()')
        super().__init__(value)
        self.value += 9


# Example 10
class GoodWay(TimesSevenCorrect, PlusNineCorrect):
    def __init__(self, value):
        print(f'{__class__}.__init__()')
        super().__init__(value)


foo = GoodWay(5)
print('Should be 7 * (5 + 9) = 98 and is', foo.value)
```

```실행결과.txt
<class '__main__.GoodWay'>.__init__()
<class '__main__.TimesSevenCorrect'>.__init__()
<class '__main__.PlusNineCorrect'>.__init__()
<class '__main__.MyBaseClass'>.__init__()
Should be 7 * (5 + 9) = 98 and is 98
```

super() 로 초기화하면 문제 없음

<br/>

**문제가 되는 케이스**

```python
class MyBaseClass:
    def __init__(self, value):
        print(f'{__class__}.__init__()')
        self.value = value


# Example 7
class TimesSeven(MyBaseClass):
    def __init__(self, value):
        print(f'{__class__}.__init__()')
        MyBaseClass.__init__(self, value)
        self.value *= 7


class PlusNine(MyBaseClass):
    def __init__(self, value):
        print(f'{__class__}.__init__()')
        MyBaseClass.__init__(self, value)
        self.value += 9


# Example 8
class ThisWay(TimesSeven, PlusNine):
    def __init__(self, value):
        print(f'{__class__}.__init__()')
        TimesSeven.__init__(self, value)
        PlusNine.__init__(self, value)


foo = ThisWay(5)
print('Should be (5 * 7) + 9 = 44 but is', foo.value)
```

```실행결과.txt
<class '__main__.ThisWay'>.__init__()
<class '__main__.TimesSeven'>.__init__()
<class '__main__.MyBaseClass'>.__init__()
<class '__main__.PlusNine'>.__init__()
<class '__main__.MyBaseClass'>.__init__()
Should be (5 * 7) + 9 = 44 but is 14
```

<br/>

---

## 10. 참고 자료

- [파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)
- [원저자 깃허브](https://github.com/bslatkin/effectivepython/blob/master/example_code/item_40.py)

<br/>

---