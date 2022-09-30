# 'Better way 50. __set_name__으로 클래스 애트리뷰트를 표시하라' 정리


## 0. 들어가며

[Effective Python 2nd 파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)을 제대로 이해하고자 블로그에 정리합니다.

{{< figure src="http://image.kyobobook.co.kr/images/book/xlarge/191/x4801165213191.jpg" width="180px" >}}

<br/>
<br/>

**현재 위치**
{{< admonition note >}}
<6. Metaclasses and Attributes>
Item 50: Annotate Class Attributes with `__set_name__`
**Better Way 50. **set\_name**으로 클래스 애트리뷰트를 표시하라**
{{< /admonition >}}


<br/>

---

## 1. 한 줄 요약 및 첨언

디스크립터에 `__set_name__`을 구현하면
디스크립터가 만들어 질 때, 해당 디스크립터를 할당 할 변수명을 미리 받을 수 있습니다.

파이썬 3.6에 도입 되었습니다.


※ 디스크립터 : 다른 클래스의 어트리뷰트로 쓰이는 클래스인데, `__set__`, `__get__`, `__del__` 중 하나라도 구현한 클래스

<br/>

---

## 2. 사용 예시

할당할 변수명을 미리 받아서, 해당 이름을 내부 애트리뷰트로 사용하는 예제입니다.

```python
# Example 11
class Field:
    def __init__(self):
        self.name = None
        self.internal_name = None

    def __set_name__(self, owner, name):
        print('__set_name__', owner, name)
        # Called on class creation for each descriptor
        self.name = name
        self.internal_name = '_' + name

    def __get__(self, instance, instance_type):
        if instance is None:
            return self
        return getattr(instance, self.internal_name, '')

    def __set__(self, instance, value):
        setattr(instance, self.internal_name, value)


# Example 12
print('\n==Before FixedCustomer==')
class FixedCustomer:
    first_name = Field()  # 요 때, __set_name__ 이 불림. name 이 first_name
    last_name = Field()
    prefix = Field()
    suffix = Field()


print('\n==Before cust==')
cust = FixedCustomer()
print(f'Before: {cust.first_name!r} {cust.__dict__}')  # Before: '' {}
cust.first_name = 'Mersenne'
print(f'After:  {cust.first_name!r} {cust.__dict__}')  # After:  'Mersenne' {'_first_name': 'Mersenne'}
```


```output.txt
==Before FixedCustomer==
__set_name__ <class '__main__.FixedCustomer'> first_name
__set_name__ <class '__main__.FixedCustomer'> last_name
__set_name__ <class '__main__.FixedCustomer'> prefix
__set_name__ <class '__main__.FixedCustomer'> suffix

==Before cust==
Before: '' {}
After:  'Mersenne' {'_first_name': 'Mersenne'}
```

<br/>

---

## 3. 기억해야 할 내용

책에서 챕터 마지막 부분에 적혀있는 내용입니다.

{{< admonition tip >}}
Metaclasses enable you to modify a class's attributes before the class is fully defined.
**메타클래스를 사용하면 어떤 클래스가 완전히 정의되기 전에 클래스의 애트리뷰트를 변경할 수 있다.**
{{< /admonition >}}

{{< admonition tip >}}
Descriptors and metaclasses make a powerful combination for declarative behavior and runtime introspection.
**디스크립터와 메타클래스를 조합하면 강력한 실행 시점 코드 검사와 선언적인 동작을 만들 수 있다.**
{{< /admonition >}}

{{< admonition tip >}}
Define `__set_name__` on your descriptor classes to allow them to take into account their surrounding class and its property names.
**`__set_name__` 특별 메서드를 디스크립터 클래스에 정의하면 디스크립터가 포함된 클래스의 프로퍼티 이름을 처리할 수 있다.**
{{< /admonition >}}

{{< admonition tip >}}
Avoid memory leaks and the `weakref` built-in module by having descriptors store data they manipulate directly within a class's instance dictionary.
**디스크립터가 변경한 클래스의 인스턴스 딕셔너리에 데이터를 저장하게 만들면 메모리 누수를 피할 수 있고, weakref 내장 메서드를 사용하지 않아도 된다.**
{{< /admonition >}}

<br/>

---

## 4. 더 알아보기

### 4.1 메타클래스를 사용했다면


옛날 방식인 듯 합니다.

```python
# Example 7
class Field:
    def __init__(self):
        # These will be assigned by the metaclass.
        self.name = None
        self.internal_name = None

    def __get__(self, instance, instance_type):
        if instance is None:
            return self
        return getattr(instance, self.internal_name, '')

    def __set__(self, instance, value):
        setattr(instance, self.internal_name, value)


# Example 5
class Meta(type):
    def __new__(meta, name, bases, class_dict):
        for key, value in class_dict.items():
            if isinstance(value, Field):
                value.name = key
                value.internal_name = '_' + key
        cls = type.__new__(meta, name, bases, class_dict)
        return cls


# Example 6
class DatabaseRow(metaclass=Meta):
    pass


# Example 8
class BetterCustomer(DatabaseRow):
    first_name = Field()
    last_name = Field()
    prefix = Field()
    suffix = Field()


# Example 9
cust = BetterCustomer()
print(f'Before: {cust.first_name!r} {cust.__dict__}')  # Before: '' {}
cust.first_name = 'Euler'
print(f'After:  {cust.first_name!r} {cust.__dict__}')  # After:  'Euler' {'_first_name': 'Euler'}
```

<br/>

---


## 10. 참고 자료

- [파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)
- [원저자 깃허브](https://github.com/bslatkin/effectivepython/blob/master/example_code/item_50.py)
- ['Better way 46. 재사용 가능한 @property 메서드를 만들려면 디스크립터를 사용하라' 정리](https://mechurak.github.io/ko/posts/python/2020-12-27_descriptor/)
- [Descriptor HowTo Guide / 파이썬 공식 문서](https://docs.python.org/3/howto/descriptor.html)


<br/>

---
