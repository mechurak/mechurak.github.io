# 'Better way 51. 합성 가능한 클래스 확장이 필요하면 메타클래스보다는 클래스 데코레이터를 사용하라' 정리


## 0. 들어가며

[Effective Python 2nd 파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)을 제대로 이해하고자 블로그에 정리합니다.

{{< figure src="http://image.kyobobook.co.kr/images/book/xlarge/191/x4801165213191.jpg" width="180px" >}}

<br/>
<br/>

**현재 위치**
{{< admonition note >}}
<6. Metaclasses and Attributes>
Item 51: Prefer Class Decorators Over Metaclasses for Compoable Class Extensions
**Better Way 51. 합성 가능한 클래스 확장이 필요하면 메타클래스보다는 클래스 데코레이터를 사용하라**
{{< /admonition >}}


<br/>

---

## 1. 한 줄 요약 및 첨언

클래스 데코레이터를 사용하면 클래스를 쉽게 확장할 수 있습니다.

<br/>

---

## 2. 사용 예시

어떤 클래스의 모든 메서드에 대해서 호출 결과를 출력하고 싶은 상황입니다.
​
['Better way 26. functools.wrap을 사용해 함수 데코레이터를 정의하라' 정리](https://mechurak.github.io/ko/posts/python/2020-12-16_function-decorator/)와 비슷하게 trace_func()을 적용하고 싶습니다.
​
```python
import types
from functools import wraps
​
​
# Example 1
def trace_func(func):
    if hasattr(func, 'tracing'):  # Only decorate once
        return func
​
    @wraps(func)
    def wrapper(*args, **kwargs):
        result = None
        try:
            result = func(*args, **kwargs)
            return result
        except Exception as e:
            result = e
            raise
        finally:
            print(f'{func.__name__}({args!r}, {kwargs!r}) -> {result!r}')  # 요걸 출력하고 싶음
​
    wrapper.tracing = True
    return wrapper
```
​
클래스 데코레이터를 적용한 예제입니다.
​
```python
# Example 4
trace_types = (
    types.MethodType,
    types.FunctionType,
    types.BuiltinFunctionType,
    types.BuiltinMethodType,
    types.MethodDescriptorType,
    types.ClassMethodDescriptorType)
​
​
# Example 9 (데코레이터 함수: 인자로 받은 클래스를 적절히 변경해서 재생성함)
def trace(klass):
    for key in dir(klass):
        value = getattr(klass, key)
        if isinstance(value, trace_types):
            wrapped = trace_func(value)
            setattr(klass, key, wrapped)
    return klass
​
​
# Example 10
@trace  # TraceDict 클래스에 @trace 클래스 데코레이터 적용
class TraceDict(dict):
    pass
```
​
TraceDict 클래스 사용 부분 입니다.
​
```python
trace_dict = TraceDict([('hi', 1)])
trace_dict['there'] = 2
temp = trace_dict['hi']
try:
    temp = trace_dict['does not exist']
except KeyError:
    pass  # Expected
else:
    assert False
```
​
실행 결과 입니다.
​
```output.txt
__new__((<class '__main__.TraceDict'>, [('hi', 1)]), {}) -> {}
__getitem__(({'hi': 1, 'there': 2}, 'hi'), {}) -> 1
__getitem__(({'hi': 1, 'there': 2}, 'does not exist'), {}) -> KeyError('does not exist')
```

<br/>

---

## 3. 기억해야 할 내용

책에서 챕터 마지막 부분에 적혀있는 내용입니다.

{{< admonition tip >}}
A class decorator is a simple function that receives a `class` instance as a parameter and returns either a new class or a modified version of the original class.
클래스 데코레이터는 class 인스턴스를 파라미터로 받아서 이 클래스를 변경한 클래스나 새로운 클래스를 반환해주는 간단한 함수다.
{{< /admonition >}}

{{< admonition tip >}}
Class decorators are useful when you want to modify every method or attribute of a class with minimal boilerplate.
준비 코드를 최소화하면서 클래스 내부의 모든 메서드나 애트리뷰트를 변경하고 싶을 때 클래스 데코레이터가 유용하다.
{{< /admonition >}}

{{< admonition tip >}}
Metaclasses can't be composed together easily, while many class decorators can be used to extend the same class without conflicts.
메타클래스는 서로 쉽게 합성할 수 없지만, 여러 클래스 데코레이터를 충돌 없이 사용해 똑같은 클래스를 확장할 수 있다.
{{< /admonition >}}

<br/>

---


## 10. 참고 자료

- [파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)
- [원저자 깃허브](https://github.com/bslatkin/effectivepython/blob/master/example_code/item_51.py)

<br/>

---
