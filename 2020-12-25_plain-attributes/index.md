# 'Better way 44. 세터와 게터 메서드 대신 평범한 애트리뷰트를 사용하라' 정리


## 0. 들어가며

[Effective Python 2nd 파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)을 제대로 이해하고자 블로그에 정리합니다.

{{< figure src="http://image.kyobobook.co.kr/images/book/xlarge/191/x4801165213191.jpg" width="180px" >}}

<br/>
<br/>

**현재 위치**
{{< admonition note >}}
<6. Metaclasses and Attributes>  
Item 44: Use Plain Attributes Instead of Setter and Getter Methods  
**Better way 44. 세터와 게터 메서드 대신 평범한 애트리뷰트를 사용하라**
{{< /admonition >}}


<br/>

---

## 1. 한 줄 요약 및 첨언

파이썬에서는 명시적인 세터나 게터 메서드를 구현할 필요가 없습니다.  
그런 기능이 필요해 진다면 @property 데코레이터를 이용합시다.


<br/>

---

## 2. 사용 예시

시작은 단순한 공개 애트리뷰트로 합니다.

```python
# Example 4
class Resistor:
    def __init__(self, ohms):
        self.voltage = 0
        self.current = 0
        self.ohms = ohms


r1 = Resistor(10e3)  # 10 * 10^3
r1.ohms += 5e3  # getter, setter 쓰지 말고, 그냥 바로 쓰면 된다.
print(r1.__dict__)  # {'voltage': 0, 'current': 0, 'ohms': 15000.0}
```

<br/>

property 데코레이터 사용한 BoundedResistance 클래스입니다.

```python
# Example 8
class BoundedResistance(Resistor):
    def __init__(self, ohms):
        super().__init__(ohms)

    @property  # getter
    def ohms(self):
        return self._ohms

    @ohms.setter  # setter
    def ohms(self, ohms):
        if ohms <= 0:  # 요런 식으로 경계값 확인도 가능
            raise ValueError(f'ohms must be > 0; got {ohms}')
        self._ohms = ohms
        self.current = self.voltage / self.ohms
```
<br>


사용 부분


```python
# Example 9
r3 = BoundedResistance(1000)
print(r3.ohms)  # 1000. 함수 이름(ohms)을 변수처럼 접근할 수 있음
print(r3.__dict__)  # {'voltage': 0, 'current': 0.0, '_ohms': 1000}. Resistor 의 ohms 는 가려짐(?)

try:
    r3.ohms = 0  # setter 불림 (0이라서 에러 발생)
except ValueError as e:
    print(e)  # ohms must be > 0; got 0
    print()


r2 = BoundedResistance(1000)
r2.voltage = 10
print(f'Before: {r2.current:.2f} amps, {r2.voltage} volts, {r2.ohms} ohms')
# > Before: 0.00 amps, 0 volts, 1000 ohms

r2.ohms = 500  # setter 불림
print(f'After: {r2.current:.2f} amps, {r2.voltage} volts, {r2.ohms} ohms')
# > After: 0.01 amps, 10 volts, 1000 ohms. current 바뀌었음
```

<br/>

---

## 3. 기억해야 할 내용

책에서 챕터 마지막 부분에 적혀있는 내용입니다.

{{< admonition tip >}}
Define new class interfaces using simple public attributes and avoid defining setter and getter methods.  
**새로운 클래스 인터페이스를 정의할 대는 간단한 공개 애트리뷰트에서 시작하고, 세터나 게터 메서드를 가급적 사용하지 말라.**
{{< /admonition >}}

{{< admonition tip >}}
Use `@property` to define special behavior when attributes are accessed on your objects, if necessary.  
**객체에 있는 애트리뷰트에 접근할 때 특별한 동작이 필요하면 `@property`로 이를 구현할 수 있다.**
{{< /admonition >}}

{{< admonition tip >}}
Follow the rule of least surprise and avoid odd side effects in your `@property` method.  
**`@property` 메서드를 만들 때는 최소 놀람의 법칙을 따르고 이상한 부작용을 만들어내지 말라.**
{{< /admonition >}}

{{< admonition tip >}}
Ensure that `@property` methods are fast; for slow or complex work — especially involving I/O or causing side effects — use normal methods instead.  
**`@property` 메서드가 빠르게 실행되도록 유지하라. 느리거나 복잡한 작업의 경우(특히 I/O를 수행하는 등의 부수 효과가 있는 경우)에는 프로퍼티 대신 일반적인 메서드를 사용하라.**
{{< /admonition >}}

<br/>

---

## 10. 참고 자료

- [파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)
- [원저자 깃허브](https://github.com/bslatkin/effectivepython/blob/master/example_code/item_44.py)
- [프로퍼티 사용하기 / 파이썬 코딩 도장](https://dojang.io/mod/page/view.php?id=2476)

<br/>

---
