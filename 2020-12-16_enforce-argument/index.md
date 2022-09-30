# 'Better way 25. 위치로만 인자를 지정하게 하거나 키워드로만 인자를 지정하게 해서 함수 호출을 명확하게 만들라' 정리


## 들어가며

[Effective Python 2nd 파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)을 제대로 이해하고자 블로그에 정리합니다.

{{< figure src="http://image.kyobobook.co.kr/images/book/xlarge/191/x4801165213191.jpg" width="180px" >}}

<br/>
<br/>

**현재 위치**
{{< admonition note >}}
<3. Functions>  
Item 25: Enforce Clarity with Keyword-Only and Positional-Only Arguments  
**Better Way 25. 위치로만 인자를 지정하게 하거나 키워드로만 인자를 지정하게 해서 함수 호출을 명확하게 만들라**
{{< /admonition >}}


<br/>

---

## 한 줄 요약 및 첨언

파라미터에 `/`, `*` 를 이용해서 위치로만 지정하는 인자와 (`/` 앞의 인자들) 키워드로만 지정하는 인자(`*` 뒤의 인자들)를 지정할 수 있습니다.

Positional-only parameters(`/` 로 구분) 은 파이썬 3.8에서 도입되었습니다.

<br/>

---

## 사용 예시

```python
# Example 16
def safe_division_e(numerator, denominator,  # '/' 앞쪽은 positional-only arguments
                    /,
                    ndigits=10,  # 사이에 있는건 둘 다 됨 (파이썬 기본)
                    *,
                    ignore_overflow=False,  # '*' 뒤쪽은 keyword-only arguments
                    ignore_zero_division=False):
    try:
        fraction = numerator / denominator
        return round(fraction, ndigits)
    except OverflowError:
        if ignore_overflow:
            return 0
        else:
            raise
    except ZeroDivisionError:
        if ignore_zero_division:
            return float('inf')
        else:
            raise


# Example 17
result = safe_division_e(22, 7)
print(result)  # 3.1428571429

result = safe_division_e(22, 7, 5)
print(result)  # 3.14286

result = safe_division_e(22, 7, ndigits=2)
print(result)  # 3.14
```


<br/>

---

## 기억해야 할 내용

책에서 챕터 마지막 부분에 적혀있는 내용입니다.

{{< admonition tip >}}
Keyword-only arguments force callers to supply certain arguments by keyword (instead of by position), which makes the intention of a function call clearer. Keyword-only arguments are defined after a single `*` in the argument list.  
**키워드로만 지정해야 하는 인자를 사용하면 호출하는 쪽에서 특정 인자를 (위치를 사용하지 않고) 반드시 키워드를 사용해 호출하도록 강제할 수 있다. 이로 인해 함수 호출의 의도를 명확히 할 수 있다. 키워드로만 지정해야 하는 인자는 인자 목록에서 `*` 다음에 위치한다.**
{{< /admonition >}}

{{< admonition tip >}}
Positional-only arguments ensure that callers can't supply certain parameters using keywords, which helps reduce coupling. Positional-only arguments are defined before a single `/` in the argument list.  
**위치로만 지정해야 하는 인자를 사용하면 호출하는 쪽에서 키워드를 사용해 인자를 지정하지 못하게 만들 수 있고, 이에 따라 함수 구현과 함수 호출 지점 사이의 결합을 줄일 수 있다. 위치로만 지정해야 하는 인자는 인자 목록에서 `/` 앞에 위치한다.**
{{< /admonition >}}

{{< admonition tip >}}
Parameters between the `/` and `*` characters in the argument list may be supplied by position or keyword, which is the default for Python parameters.  
**인자 목록에서 `/`와 `*` 사이에 있는 파라미터는 키워드를 사용해 전달해도 되고 위치를 기반으로 전달해도 된다. 이런 동작은 파이썬 함수 파라미터의 기본 동작이다.**
{{< /admonition >}}

<br/>

---

## 참고 자료

- [파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)
- [길벗출판사 깃허브](https://github.com/gilbutITbook/080235/blob/master/Chapter3/Better%20way25.py)
- [원저자 깃허브](https://github.com/bslatkin/effectivepython/blob/master/example_code/item_25.py)
- [What’s New In Python 3.8 / 파이썬 공식 문서](https://docs.python.org/3.8/whatsnew/3.8.html#positional-only-parameters)
- [PEP 570 -- Python Positional-Only Parameters](https://www.python.org/dev/peps/pep-0570/)
- [PEP 3102 -- Keyword-Only Arguments](https://www.python.org/dev/peps/pep-3102/)

<br/>

---
