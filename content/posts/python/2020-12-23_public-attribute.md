---
title: "'Better way 42. 비공개 애트리뷰트보다는 공개 애트리뷰트를 사용하라' 정리"
date: 2020-12-23T01:02:00+09:00
summary: "비공개 애트리뷰트(변수 앞에 밑줄 두 개)는 사용하지 맙시다."
categories: [ python ]
series: [ effective python 5. 클래스와 인터페이스 ]
series_weight: 42
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
Item 42: Prefer Public Attributes Over Private Ones  
**Better way 42. 비공개 애트리뷰트보다는 공개 애트리뷰트를 사용하라**
{{< /admonition >}}


<br/>

---

## 1. 한 줄 요약 및 첨언

비공개 애트리뷰트(변수 앞에 밑줄 두 개)는 사용하지 맙시다.


<br/>

---

## 2. 사용 예시

조심해야할 필드라면 해당 필드에 문서를 남겨 놓읍시다.


```python
# Example 13
class MyStringClass:
    def __init__(self, value):
        # This stores the user-supplied value for the object.
        # It should be coercible to a string. Once assigned in
        # the object it should be treated as immutable.
        self._value = value

    def get_value(self):
        return str(self._value)


class MyIntegerSubclass(MyStringClass):
    def get_value(self):
        return self._value


foo = MyIntegerSubclass(5)
assert foo.get_value() == 5
```

<br/>

---

## 3. 기억해야 할 내용

책에서 챕터 마지막 부분에 적혀있는 내용입니다.

{{< admonition tip >}}
Private attributes aren't rigorously enforced by the Python compiler.  
**파이썬 컴파일러는 비공개 애트리뷰트를 자식 클래스나 클래스 외부에서 사용하지 못하도록 엄격히 금지하지 않는다.**
{{< /admonition >}}

{{< admonition tip >}}
Plan from the beginning to allow subclasses to do more with your internal APIs and attributes instead of choosing to lock them out.  
**여러분의 내부 API에 있는 클래스의 하위 클래스를 정의하는 사람들이 여러분이 제공하는 클래스의 애트리뷰트를 사용하지 못하도록 막기보다는 애트리뷰트를 사용해 더 많은 일을 할 수 있게 허용하라.**
{{< /admonition >}}

{{< admonition tip >}}
Use documentation of protected fields to guide subclasses instead of trying to force access control with private attributes.  
**비공개 애트리뷰트로 (외부나 하위 클래스의) 접근을 막으려고 시도하기보다는 보호된 필드를 사용하면서 문서에 적절한 가이드를 남겨라.**
{{< /admonition >}}

{{< admonition tip >}}
Only consider using private attribute to avoid naming conflicts with subclasses that are out or your control.  
**여러분이 코드 작성을 제어할 수 없는 하위 클래스에서 이름 충돌이 일어나는 경우를 막고 싶을 때만 비공개 애트리뷰트를 사용할 것을 권한다.**
{{< /admonition >}}

<br/>

---

## 4. 더 알아보기

### 4.1 비공개 애트리뷰트 접근

내부적으로 하위 클래스에서 이름이 바뀝니다.  
그래도 어차피 접근할 수 있습니다.

```python
# Example 6
class MyParentObject:
    def __init__(self):
        self.__private_field = 71


class MyChildObject(MyParentObject):
    def get_private_field(self):
        return self.__private_field


baz = MyChildObject()
try:
    baz.get_private_field()  # 요건 예외 발생
except AttributeError:
    print('Expected AttributeError')  # Expected AttributeError

# Example 7  (그래도 접근 가능)
print(baz._MyParentObject__private_field)  # 71

# Example 8 (__dict__ 로 인스턴스의 애트리뷰트 확인)
print(baz.__dict__)  # {'_MyParentObject__private_field': 71}
```

<br/>

---

## 10. 참고 자료

- [파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)
- [원저자 깃허브](https://github.com/bslatkin/effectivepython/blob/master/example_code/item_42.py)

<br/>

---