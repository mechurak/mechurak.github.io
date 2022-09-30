# 'Better way 46. 재사용 가능한 @property 메서드를 만들려면 디스크립터를 사용하라' 정리


## 0. 들어가며

[Effective Python 2nd 파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)을 제대로 이해하고자 블로그에 정리합니다.

{{< figure src="http://image.kyobobook.co.kr/images/book/xlarge/191/x4801165213191.jpg" width="180px" >}}

<br/>
<br/>

**현재 위치**
{{< admonition note >}}
<6. Metaclasses and Attributes>  
Item 46: Use Descriptors for Reusable @property Methods  
**Better way 46. 재사용 가능한 `@property` 메서드를 만들려면 디스크립터를 사용하라**
{{< /admonition >}}


<br/>

---

## 1. 한 줄 요약 및 첨언

다른 클래스의 클래스 어트리뷰트로 쓰이는 클래스인데 `__set__`, `__get__`, `__del__` 중 하나라도 구현한 클래스를 디스크립터라고 합니다.  
경계값 체크 등에 사용할 수 있을 듯 합니다.


<br/>

---

## 2. 사용 예시

Grade 는 `__get__`, `__set__`을 구현한 디스크립터입니다.

```python
# Example 14
from weakref import WeakKeyDictionary


class Grade:
    def __init__(self):
        # 다른 인스턴스(first_exam, second_exam)에 다른 값을 저장하기 위해
        # 더하기, 메모리릭을 피하기 위해 (그냥 {} 으로 하면, __set__ 에서 인자로 받은 instance 가 안 없어짐)
        self._values = WeakKeyDictionary()

    def __get__(self, instance, instance_type):  # 호출한 인스턴스도 받음
        if instance is None:
            return self
        return self._values.get(instance, 0)

    def __set__(self, instance, value):  # 호출한 인스턴스도 받음
        if not (0 <= value <= 100):
            raise ValueError('Grade must be between 0 and 100')
        self._values[instance] = value
```


<br>

사용하는 예제 입니다.

```python
class Exam:
    math_grade = Grade()  # 클래스 애트리뷰트라 한 번만 초기화
    writing_grade = Grade()
    science_grade = Grade()
    # def __init__(self):  # 요렇게는 디스크립터가 동작하지 않음
    #     self.math_grade = Grade()
    #     self.writing_grade = Grade()
    #     self.science_grade = Grade()


first_exam = Exam()
print(first_exam.__dict__)  # {}
print(Exam.__dict__)  # {'math_grade': <__main__.Grade object at 0x000002A0D1999730>, ... }
first_exam.writing_grade = 82
try:
    first_exam.math_grade = 120
except ValueError as e:
    print("ValueError!", e)  # ValueError! Grade must be between 0 and 100

second_exam = Exam()
second_exam.writing_grade = 75

print(f'First {first_exam.writing_grade} is right')  # First 82 is right
print(f'Second {second_exam.writing_grade} is right')  # Second 75 is right
```

<br/>

---

## 3. 기억해야 할 내용

책에서 챕터 마지막 부분에 적혀있는 내용입니다.

{{< admonition tip >}}
Reuse the behavior and validation of @property methods by defining your own descriptor classes.  
**`@property` 메서드의 동작과 검증 기능을 재사용하고 싶다면 디스크립터 클래스를 만들라.**
{{< /admonition >}}

{{< admonition tip >}}
Use `WeakKeyDictionary` to ensure that your descriptor classes don't cause memory leaks.  
**디스크립터 클래스를 만들 때는 메모리 누수를 방지하기 위해 WeakDictionary를 사용하라.**
{{< /admonition >}}

{{< admonition tip >}}
Don't get bogged down trying to understand exactly how `__getattribute__` uses the descriptor protocol for getting and setting attributes.  
**`__getattribute__`가 디스크립터 프로토콜을 사용해 애트리뷰트 값을 읽거나 설정하는 방식을 정확히 이해하라.**
{{< /admonition >}}

<br/>

---

## 4. 더 알아보기

### 4.1 디스크립터 애트리뷰트에 대한 접근 해석

디스크립터 애트리뷰트에 대한 접근을 파이썬이 어떻게 처리하는지 보여주는 예제입니다.

```python
third_exam = Exam()

# 요 코드는
third_exam.writing_grade = 40

# 요렇게 해석됨
Exam.__dict__['writing_grade'].__set__(third_exam, 40)


# 요 코드는
temp = third_exam.writing_grade
print(temp)  # 40

# 요렇게 해석됨
temp = Exam.__dict__['writing_grade'].__get__(third_exam, Exam)
print(temp)  # 40
```

`__getattribute__` 메서드가 요 동작을 이끌어 낸다고 합니다. 47장에 나오나 부네요.  
Exam 클래스의 인스턴스(`third_exam`)에 `writing_grade`라는 이름의 애트리뷰트가 없으면, 파이썬은 Exam 클래스의 애트리뷰트를 대신 사용합니다. 이 클래스의 애트리뷰트가 `__get__`과 `__set__` 메서드가 정의된 객체라면 파이썬은 디스크립터 프로토콜을 따라야 한다고 결정한다고 합니다.

<br/>

---


## 10. 참고 자료

- [파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)
- [원저자 깃허브](https://github.com/bslatkin/effectivepython/blob/master/example_code/item_46.py)
- [[코드잇] 쉽게 배우는 파이썬 문법 - 프로퍼티(Property) 3편](https://blog.naver.com/codeitofficial/221701646124)

<br/>

---
