# 'Better way 26. functools.wrap을 사용해 함수 데코레이터를 정의하라' 정리


## 들어가며

[Effective Python 2nd 파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)을 제대로 이해하고자 블로그에 정리합니다.

{{< figure src="http://image.kyobobook.co.kr/images/book/xlarge/191/x4801165213191.jpg" width="180px" >}}

<br/>
<br/>

**현재 위치**
{{< admonition note >}}
<3. Functions>  
Item 26: Define Function Decorators with `functools.wraps`  
**Better way 26. functools.wrap을 사용해 함수 데코레이터를 정의하라**
{{< /admonition >}}


<br/>

---

## 한 줄 요약 및 첨언

데코레이터를 직접 구현할 일이 생기면 functools 의 wraps 데코레이터를 사용해야 합니다.

<br/>

---

## 사용 예시

```python
import pickle
from functools import wraps


# Example 8
def trace(func):
    @wraps(func)  # 요걸 사용하라 (func의 중요한 메타데이터를 복사해 리턴할 함수에 적용해 줌)
    def wrapper(*args, **kwargs):
        result = func(*args, **kwargs)
        print(f'{func.__name__}({args!r}, {kwargs!r}) '
              f'-> {result!r}')
        return result

    return wrapper


@trace
def fibonacci(n):
    """Return the n-th Fibonacci number"""
    if n in (0, 1):
        return n
    return fibonacci(n - 2) + fibonacci(n - 1)


# help 가 원하는 데로 동작 함
help(fibonacci)
## Help on function fibonacci in module __main__:
##
## fibonacci(n)
##    Return the n-th Fibonacci number

# pickle 객체 직렬화도 제대로 작동함
print(pickle.dumps(fibonacci))
## b'\x80\x04\x95\x1a\x00\x00\x00\x00\x00\x00\x00\x8c\x08__main__\x94\x8c\tfibonacci\x94\x93\x94.'

# 함수 사용
print(fibonacci(2))
## fibonacci((0,), {}) -> 0
## fibonacci((1,), {}) -> 1
## fibonacci((2,), {}) -> 1
## 1
```


<br/>

---

## 기억해야 할 내용

책에서 챕터 마지막 부분에 적혀있는 내용입니다.

{{< admonition tip >}}
Decorators in Python are syntax to allow one function to modify another function at runtime.  
**파이썬 데코레이터는 실행 시점에 함수가 다른 함수를 변경할 수 있게 해주는 구문이다.**
{{< /admonition >}}

{{< admonition tip >}}
Using decorators can cause strange behaviors in tools that do introspection, such as debuggers.  
**데코레이터를 사용하면 디버거 등 인트로스펙션을 사용하는 도구가 잘못 작동할 수 있다.**
{{< /admonition >}}

{{< admonition tip >}}
Use the `wraps` decorator from the `functools` built-in module when you define your own decorators to avoid issues.  
**직접 데코레이터를 구현할 때 인트로스펙션에서 문제가 생기지 않길 바란다면 functools 내장 모듈의 wraps 데코레이터를 사용하라.**
{{< /admonition >}}

<br/>

---


## 데코레이터의 동작

`@` 기호를 사용하는 것은 해당 함수를 인자로 데코레이터 호출 후, 반환한 결과를 원래 이름으로 등록하는 동작을 합니다.

```python
# Example 1
def trace(func):
    def wrapper(*args, **kwargs):
        print(f'before {func.__name__}({args!r}, {kwargs!r})')
        result = func(*args, **kwargs)
        print(f'after {func.__name__}({args!r}, {kwargs!r}) -> {result!r}')
        return result

    return wrapper


# Example 3
def fibonacci(n):
    """Return the n-th Fibonacci number"""
    if n in (0, 1):
        return n
    return fibonacci(n - 2) + fibonacci(n - 1)

# '@' 기호를 사용하는 것은 아래와 같은 의미 (데코레이터 호출 후, 반환한 결과를 원래 이름으로 등록)
fibonacci = trace(fibonacci)

fibonacci(2)
## before fibonacci((2,), {})
## before fibonacci((0,), {})
## after fibonacci((0,), {}) -> 0
## before fibonacci((1,), {})
## after fibonacci((1,), {}) -> 1
## after fibonacci((2,), {}) -> 1
```

<br/>

---

## 의도하지 않은 상황

원래의 fibonacci 가 다른 객체(wrapper)로 바뀐 상황이 되버려서, help, pickle 등이 적절하게 동작하지 않게 됩니다. 그래서 functools.wrap 를 사용해야 합니다.

```python
# Example 1
import pickle


def trace(func):
    def wrapper(*args, **kwargs):
        print(f'before {func.__name__}({args!r}, {kwargs!r})')
        result = func(*args, **kwargs)
        print(f'after {func.__name__}({args!r}, {kwargs!r}) -> {result!r}')
        return result

    return wrapper


# Example 2
@trace
def fibonacci(n):
    """Return the n-th Fibonacci number"""
    if n in (0, 1):
        return n
    return (fibonacci(n - 2) + fibonacci(n - 1))


fibonacci(2)
## before fibonacci((2,), {})
## before fibonacci((0,), {})
## after fibonacci((0,), {}) -> 0
## before fibonacci((1,), {})
## after fibonacci((1,), {}) -> 1
## after fibonacci((2,), {}) -> 1


print(fibonacci)
## <function trace.<locals>.wrapper at 0x000001D4B7CFC790>

help(fibonacci)
## Help on function wrapper in module __main__:
## 
## wrapper(*args, **kwargs)

pickle.dumps(fibonacci)
## Traceback ...
## AttributeError: Can't pickle local object 'trace.<locals>.wrapper'
```

<br/>

---

## 참고 자료

- [파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)
- [길벗출판사 깃허브](https://github.com/gilbutITbook/080235/blob/master/Chapter3/Better%20way26.py)
- [원저자 깃허브](https://github.com/bslatkin/effectivepython/blob/master/example_code/item_26.py)

<br/>

---
