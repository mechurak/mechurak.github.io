---
title: "'Better way 47. 지연 계산 애트리뷰트가 필요하면, __getattr__, __getattribute__, __setattr__을 사용하라' 정리"
date: 2020-12-28T01:00:00+09:00
summary: "object hook을 사용해 특별한 일을 추가할 수 있습니다."
categories: [ python ]
series: [ effective python 6. 메타클래스와 애트리뷰트 ]
series_weight: 47
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
Item 47: Use `__getattr__`, `__getattribute__`, and `__setattr__` for Lazy Attrubutes  
**Better way 47. 지연 계산 애트리뷰트가 필요하면, `__getattr__`, `__getattribute__`, `__setattr__`을 사용하라**
{{< /admonition >}}


<br/>

---

## 1. 한 줄 요약 및 첨언

`__getattr__`, `__getattribute__`, `__setattr__` 를 사용하면, 애트리뷰트 접근 및 세팅때 특별한 일을 수행할 수 있습니다.  
요런 애들을 object hook 이라고 한다는군요.


<br/>

---

## 2. 사용 예시

### 2.1 **getattr**

객체의 인스턴스 딕셔너리에 없는 애트리뷰트에 접근할 때 뭔가 처리해야 한다면 `__getattr__`를 구현합니다.

```python
# Example 1
class LazyRecord:
    def __init__(self):
        self.exists = 5

    def __getattr__(self, name):
        print(f'* Called __getattr__({name!r}), populating instance dictionary')
        value = f'Value for {name}'
        setattr(self, name, value)
        print(f'* Returning {value!r}')
        return value


# Example 2
data = LazyRecord()
print('Before:', data.__dict__)  # Before: {'exists': 5}
print('First foo:   ', data.foo)
print('After: ', data.__dict__)  # After:  {'exists': 5, 'foo': 'Value for foo'}
print('Second foo:   ', data.foo)
print('Has peach: ', hasattr(data, 'peach'))  # Has peach:  True
```

<br>

```output.txt
Before: {'exists': 5}

* Called __getattr__('foo'), populating instance dictionary
* Returning 'Value for foo'
First foo:    Value for foo

After:  {'exists': 5, 'foo': 'Value for foo'}

Second foo:    Value for foo

* Called __getattr__('peach'), populating instance dictionary
* Returning 'Value for peach'
Has peach:  True
```

First foo 때만 `__getattr__`가 호출되었고, 인스턴스 딕셔너리에 'foo'가 추가 되었습니다.  
`hasattr`를 호출해도 `__getattr__`가 호출되었습니다.

<br>

### 2.2 **getattribute**

애트리뷰트에 접근할 때마다 뭔가 처리해야 한다면 `__getattribute__`를 구현합니다.

```python
# Example 4
class ValidatingRecord:
    def __init__(self):
        self.exists = 5

    def __getattribute__(self, name):
        print(f'* Called __getattribute__({name!r})')
        try:
            value = super().__getattribute__(name)  # 실제 값을 가져올 때는 super() 로
            # value = self.name  # 요런식이면 또 __getattribute__를 불러스 무한 재귀에 빠짐
            print(f'* Found {name!r}, returning {value!r}')
            return value
        except AttributeError:
            value = f'Value for {name}'
            print(f'* Setting {name!r} to {value!r}')
            setattr(self, name, value)
            return value


data = ValidatingRecord()
print('exists:     ', data.exists)  # exists:      5
print('First foo:  ', data.foo)  # First foo:   Value for foo
print('Second foo: ', data.foo)  # Second foo:  Value for foo
print('Has foo: ', hasattr(data, 'foo'))  # Has foo:  True
```

실제 값을 가져올 때는 `value = super().__getattribute__(name)` 요렇게 super 를 사용해야 합니다.

```output.txt
* Called __getattribute__('exists')
* Found 'exists', returning 5
exists:      5

* Called __getattribute__('foo')
* Setting 'foo' to 'Value for foo'
First foo:   Value for foo

* Called __getattribute__('foo')
* Found 'foo', returning 'Value for foo'
Second foo:  Value for foo

* Called __getattribute__('foo')
* Found 'foo', returning 'Value for foo'
Has foo:  True
```

애트리뷰트에 접근할 때마다 `__getattribute__`가 호출됩니다.  
`hasattr()`도 마찬가지 입니다.

<br>

### 2.3 **setattr**

인스턴스의 애트리뷰트 값을 설정할 때마다 뭔가 처리해야 한다면 `__setattr__`를 구현합니다.

```python
# Example 9
class LoggingSavingRecord:
    def __setattr__(self, name, value):
        print(f'* Called __setattr__({name!r}, {value!r})')
        super().__setattr__(name, value)


data = LoggingSavingRecord()
print('Before: ', data.__dict__)  # Before:  {}
data.foo = 5
print('After:  ', data.__dict__)  # After:   {'foo': 5}
data.foo = 7
print('Finally:', data.__dict__)  # Finally: {'foo': 7}
```

실제 값을 세팅할 때는 `super().__setattr__(name, value)` 요렇게 super 를 사용해야 합니다.

```output.txt
Before:  {}
* Called __setattr__('foo', 5)
After:   {'foo': 5}
* Called __setattr__('foo', 7)
Finally: {'foo': 7}
```

<br/>

---

## 3. 기억해야 할 내용

책에서 챕터 마지막 부분에 적혀있는 내용입니다.

{{< admonition tip >}}
Use `__getattr__` and `__setattr__` to lazily load and save attributes for an object.  
**`__getattr__`과 `__setattr__`을 사용해 객체의 애트리뷰트를 지연해 가져오거나 저장할 수 있다.**
{{< /admonition >}}

{{< admonition tip >}}
Understand that `__getattr__` only gets called when accessing a missing attribute, whereas `__getattribute__` gets called every time any attribute is accessed.  
`__getattr__`은 애트리뷰트가 존재하지 않을 때만 호출되지만, `__getattribute__`는 애트리뷰트를 읽을 때마다 항상 호출된다는 점을 이해하라.
{{< /admonition >}}

{{< admonition tip >}}
Avoid infinite recursion in `__getattribute__` and `__setattr__` by using methods from `super()` (i.e., the `object` class) to access instance attributes.  
`__getattribute__`와 `__setattr__`에서 무한 재귀를 피하려면 `super()`에 있는(즉, object 클래스에 있는) 메서드를 사용해 인스턴스 애트리뷰트에 접근하라.
{{< /admonition >}}

<br/>

---

## 10. 참고 자료

- [파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)
- [원저자 깃허브](https://github.com/bslatkin/effectivepython/blob/master/example_code/item_47.py)

<br/>

---