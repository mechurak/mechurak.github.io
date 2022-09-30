# 'Better way 24. None과 독스트링을 사용해 동적인 디폴트 인자를 지정하라' 정리


## 들어가며

[Effective Python 2nd 파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)을 제대로 이해하고자 블로그에 정리합니다.

{{< figure src="http://image.kyobobook.co.kr/images/book/xlarge/191/x4801165213191.jpg" width="180px" >}}

<br/>
<br/>

**현재 위치**
{{< admonition note >}}
<3. Functions>  
Item 24: Use None and Docstrings to Specify Dynamic Default Arguments  
**Better Way 24. None과 독스트링을 사용해 동적인 디폴트 인자를 지정하라**
{{< /admonition >}}


<br/>

---


## 한 줄 요약 및 첨언

디폴트 값을 지정할 때, `[]`, `{}` 요런 mutable 한 애들은 쓰면 안됩니다. `None`을 사용하고, 함수 안에서 채웁시다.

<br/>

---

## 사용 예시

```python
# Example 2
def log(message, when=None):
    """Log a message with a timestamp.
    Args:
        message: Message to print.
        when: datetime of when the message occurred.
            Defaults to the present time.
    """
    if when is None:
        when = datetime.now()
    print(f'{when}: {message}')


# Example 3
log('Hi there!')
sleep(0.1)
log('Hello again!')
```


<br/>

---

## 기억해야 할 내용

책에서 챕터 마지막 부분에 적혀있는 내용입니다.

{{< admonition tip >}}
A default argument value is evaluated only once: during function definition at module load time. This can cause odd behaviors for dynamic values (like `{}`, `[]` or `datetime.now()`).  
**디폴트 인자 값은 그 인자가 포함된 함수 정의가 속한 모듈이 로드되는 시점에 단 한번만 평가된다. 이로 인해 동적인 값(`{}`, `[]`, `datetime.now()` 등)의 경우 이상한 동작이 일어날 수 있다.**
{{< /admonition >}}

{{< admonition tip >}}
Use `None` as the default value for any keyword argument that has a dynamic value. Document the actual default behavior in the function's docsting.  
**동적인 값을 가질 수 있는 키워드 인자의 디폴트 값을 표현할 때는 None을 사용하라. 그리고 함수의 독스트링에 실제 동적인 디폴트 인자가 어떻게 동작하는지 문서화해두라.**
{{< /admonition >}}

{{< admonition tip >}}
Using `None` to represent keyword argument default values also works correctly with type annotations.  
**타입 애너테이션을 사용할 때도 None을 사용해 키워드 인자의 디폴트 값을 표현하는 방식을 적용할 수 있다.**
{{< /admonition >}}

<br/>

---

## 에러 케이스 

```python
# Example 4
import json


def decode(data, default={}):  # Default argument value is mutable
    try:
        return json.loads(data)
    except ValueError:
        return default


# Example 5
foo = decode('bad data')
foo['stuff'] = 5
bar = decode('also bad')
bar['meep'] = 1
print('Foo:', foo)  # Foo: {'stuff': 5, 'meep': 1}
print('Bar:', bar)  # Bar: {'stuff': 5, 'meep': 1}

# Example 6
assert foo is bar  # 같은 객체임
```

디폴트 값은 (모듈을 로드하는 시점에) 한 번만 평가되고, 그 딕셔너리가 호출 때마다 공유되게 됩니다.

<br/>

---

## 참고 자료

- [파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)
- [길벗출판사 깃허브](https://github.com/gilbutITbook/080235/blob/master/Chapter3/Better%20way24.py)
- [원작자 깃허브](https://github.com/bslatkin/effectivepython/blob/master/example_code/item_24.py)

<br/>

---
