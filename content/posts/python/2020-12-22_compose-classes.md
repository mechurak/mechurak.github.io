---
title: "'Better way 37. 내장 타입을 여러 단계로 내포시키기보다는 클래스를 합성하라' 정리"
date: 2020-12-22T01:00:00+09:00
summary: "값을 관리하는 부분이 점점 복잡해지고 있다면, 해당 기능을 클래스로 분리합시다."
categories: [ python ]
series: [ effective python 5. 클래스와 인터페이스 ]
series_weight: 37
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
Item 37: Compose Classes Instead of Nesting Many Levels of Built-in Types  
**Better way 37. 내장 타입을 여러 단계로 내포시키기보다는 클래스를 합성하라**
{{< /admonition >}}


<br/>

---

## 1. 한 줄 요약 및 첨언

내포 단계가 두 단계 이상이 되면 더 이상 딕셔너리, 리스트, 튜플 계층을 추가하지 말아야 합니다.  
다른 사람이 이해하기 어려워지고, 조금만 지나도 기억나지 않을 겁니다.

값을 관리하는 부분이 점점 복잡해지고 있다면, 해당 기능을 클래스로 분리합시다.

<br/>

---

## 2. 사용 예시

과목별로 여러번 시험을 보고, 그 시험마다 가중치가 다를 수 있는 상황입니다.  
namedtuple을 사용하는 Subject 클래스 정의했습니다.

```python
from collections import defaultdict, namedtuple

# namedtuple을 사용하면 작은 불변 데이터 클래스를 쉽게 정의할 수 있음
Grade = namedtuple('Grade', ('score', 'weight'))


# Example 12
class Subject:
    def __init__(self):
        self._grades = []

    def report_grade(self, score, weight):
        self._grades.append(Grade(score, weight))

    def average_grade(self):
        total, total_weight = 0, 0
        for grade in self._grades:
            total += grade.score * grade.weight
            total_weight += grade.weight
        return total / total_weight
```

<br/>

Student는 Subject를 value 로 갖는 defaultdict을 가지고 있고,  
Gradebook은 Student를 value로 갖는 defaultdict을 가지고 있습니다.

```python
# Example 13
class Student:
    def __init__(self):
        self._subjects = defaultdict(Subject)

    def get_subject(self, name):
        return self._subjects[name]

    def average_grade(self):
        total, count = 0, 0
        for subject in self._subjects.values():
            total += subject.average_grade()
            count += 1
        return total / count


# Example 14
class Gradebook:
    def __init__(self):
        self._students = defaultdict(Student)

    def get_student(self, name):
        return self._students[name]
```

<br/>

클래스 사용

```python
# Example 15
book = Gradebook()
albert = book.get_student('Albert Einstein')
math = albert.get_subject('Math')
math.report_grade(75, 0.05)
math.report_grade(65, 0.15)
math.report_grade(70, 0.80)
print('Albert - Math:', math.average_grade())  # Albert - Math: 69.5

gym = albert.get_subject('Gym')
gym.report_grade(100, 0.40)
gym.report_grade(85, 0.60)
print('Albert - Gym:', gym.average_grade())  # Albert - Gym: 91.0

print('Albert - Total:', albert.average_grade())  # Albert - Total: 80.25
```

<br/>

---

## 3. 기억해야 할 내용

책에서 챕터 마지막 부분에 적혀있는 내용입니다.

{{< admonition tip >}}
Avoid making dictionaries with values that are dictionaries, long tuples, or complex nesting of other built-in types.  
**딕셔너리, 긴 튜플, 다른 내장 타입이 복잡하게 내포된 데이터를 값으로 사용하는 딕셔너리를 만들지 말라.**
{{< /admonition >}}

{{< admonition tip >}}
Use `namedtuple` for lightweight, immutable data containers before you need the flexibility of a full class.  
**완전한 클래스가 제공하는 유연성이 필요하지 않고 가벼운 불변 데이터 컨테이너가 필요하다면 namedtuple을 사용하라.**
{{< /admonition >}}

{{< admonition tip >}}
Move your bookkeeping code to using multiple classes when your internal state dictionaries get complicated.  
**내부 상태를 표현하는 딕셔너리가 복잡해지면 이 데이터를 관리하는 코드를 여러 클래스로 나눠서 재작성하라.**
{{< /admonition >}}

<br/>

---


## 4. 더 알아보기

### 4.1 namedtuple의 한계

namedtuple이 유용한 상황이 많지만, 아닌 경우도 많다고 합니다.

> namedtuple 클래스는 디폴트 인자 값을 지정할 수 없다. 따라서 선택적인 프로퍼티가 많은 데이터에 namedtuple을 사용하기는 어렵다. 프로퍼티가 4~5개보다 더 많아지면 dataclasses 내장 모듈을 사용하는 편이 낫다.

> 여전히 namedtuple 인스턴스의 애트리뷰트 값을 숫자 인덱스를 사용해 접근할 수 있도 이터레이션도 가능하다. 특히 외부에 제공하는 API의 경우 이런 특성으로 인해 나중에 namedtuple을 실제 클래스로 변경하기 어려울 수도 있다. 여러분이 namedtuple을 사용하는 모든 부분을 제어할 수 있는 상황이 아니라면 명시적으로 새로운 클래스를 정의하는 편이 더 낫다.

<br/>

---

## 10. 참고 자료

- [파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)
- [원저자 깃허브](https://github.com/bslatkin/effectivepython/blob/master/example_code/item_37.py)

<br/>

---