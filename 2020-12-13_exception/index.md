# 'Better way 20. None을 반환하기보다는 예외를 발생시켜라' 정리


## 들어가며

[Effective Python 2nd 파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)을 제대로 이해하고자 블로그에 정리합니다.

{{< figure src="http://image.kyobobook.co.kr/images/book/xlarge/191/x4801165213191.jpg" width="180px" >}}

<br/>
<br/>

**현재 위치**
{{< admonition note >}}
<3. Functions>  
Item 20: Prefer Raising Exceptions to Returning None  
**Better Way 20. None을 반환하기보다는 예외를 발생시켜라**
{{< /admonition >}}


<br/>

---


## 한 줄 요약 및 첨언

에러의 의미로 `None`을 리턴하면 명확하지 않을 수 있습니다. (사용처에서 if문으로 검사할 경우 False 와 같은 의미가 되서, 잘 못 해석할 수 있습니다.)  
Exception 을 발생시켜서 에러 상황을 명확히 하는 게 낫다고 하네요.  
Docstring 잘 써 주고, 타입 힌트도 적어주도록 합시다.

<br/>

---

## 사용 예시

```python
# 파라미터와 리턴 타입 표시해줌 (float 이 리턴 타입이니까 None 은 안오겠군)
def careful_divide(a: float, b: float) -> float:
    """Divides a by b.
    Raises:
        ValueError: When the inputs cannot be divided.
    """
    try:
        return a / b
    except ZeroDivisionError as e:
        raise ValueError('Invalid inputs')

# 사용처
try:
    result = careful_divide(1, 0)
    assert False
except ValueError:
    pass  # Expected

assert careful_divide(1, 5) == 0.2
```


<br/>

---

## 기억해야 할 내용

책에서 챕터 마지막 부분에 적혀있는 내용입니다.

{{< admonition tip >}}
Functions that return `None` to indicate special meaning are error prone because None and other values (e.g., zero, the empty string) all evaluate to False in conditional expressions.  
**특별한 의미를 표시하는 None을 반환하는 함수를 사용하면 None과 다른 값(예: 0이나 빈 문자열)이 조건문에서 False로 평가될 수 있기 때문에 실수하기 쉽다.**
{{< /admonition >}}

{{< admonition tip >}}
Raise exceptions to indicate special situations instead of returning `None`. Expect the calling code to handle exceptions properly when they're documented.  
**특별한 상황을 표현하기 위해 None을 반환하는 대신 예외를 발생시켜라. 문서에 예외 정보를 기록해 호출자가 예외를 제대로 처리하도록 하라.**
{{< /admonition >}}

{{< admonition tip >}}
Type annotations can be used to make it clear that a function will never return the value `None`, even in special situations.  
**함수가 특별한 경우를 포함하는 그 어떤 경우에도 절대로 None을 반환하지 않는다는 사실을 타입 애너테이션으로 명시할 수 있다.**
{{< /admonition >}}

<br/>

---

## 참고 자료

- [파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)
- [길벗출판사 깃허브](https://github.com/gilbutITbook/080235/blob/master/Chapter3/Better%20way20.py)
- [원저자 깃허브](https://github.com/bslatkin/effectivepython/blob/master/example_code/item_20.py)

<br/>

---
