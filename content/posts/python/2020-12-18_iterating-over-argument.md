---
title: "'Better way 31. 인자에 대해 이터레이션 할 때는 방어적이 되라' 정리"
date: 2020-12-18T01:00:00+09:00
summary: "인자로 이터레이터를 받을 때는 주의해야 합니다. (이미 사용한 이터레이터와 원래 비어있는 이터레이터를 구분 불가)"
categories: [ python ]
series: [ effective python 4. 컴프리헨션과 제너레이터 ]
series_weight: 31
# featuredImage: http://image.kyobobook.co.kr/images/book/xlarge/191/x4801165213191.jpg
tags:
- python
---

## 들어가며

[Effective Python 2nd 파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)을 제대로 이해하고자 블로그에 정리합니다.

{{< figure src="http://image.kyobobook.co.kr/images/book/xlarge/191/x4801165213191.jpg" width="180px" >}}

<br/>
<br/>

**현재 위치**
{{< admonition note >}}
<4. Comprehensions and Generators>  
Item 31: Be Defensive When Iterating Over Arguments  
**Better way 31. 인자에 대해 이터레이션 할 때는 방어적이 되라**
{{< /admonition >}}


<br/>

---

## 한 줄 요약 및 첨언

인자로 이터레이터를 받을 때는 주의해야 합니다. (이미 사용한 이터레이터와 원래 비어있는 이터레이터를 구분 불가)  
한 번 사용한 이터레이터는 (소진된 상태 이므로) 다시 사용할 수 없습니다.

같은 항목들에 대해서 순회가 필요하면, 필요할 때마다 새로운 이터레이터를 만들거나, 이터레이터 프로토콜(iterator protocol)을 구현한 새로운 컨테이터 클래스를 만들어 사용할 수 있습니다. (후자를 추천)

<br/>

---

## 사용 예시

이터레이터 프로토콜을 구현한 컨테이너 클래스 사용(Example 10)

```python
# Example 1 (받은 인자로 두 번 순회 수행함)
def normalize(numbers):
    total = sum(numbers)  # 한 번
    result = []
    for value in numbers:  # 두 번 (numbers 가 이터레이터면 위에서 소진 되었음)
        percent = 100 * value / total
        result.append(percent)
    return result


# Example 10 (이터레이터 프로토콜을 구현한 컨테이너 클래스)
class ReadVisits:
    def __init__(self, data_path):
        self.data_path = data_path

    def __iter__(self):  # 요걸 제너레이터로 구현. for 구문에 쓰일 때 요게 사용됨 
        with open(self.data_path) as f:
            for line in f:
                yield int(line)


path = 'my_numbers.txt'
# 15
# 35
# 80


# Example 11
visits = ReadVisits(path)
percentages = normalize(visits)
print(percentages)  # [11.538461538461538, 26.923076923076923, 61.53846153846154]
assert sum(percentages) == 100.0
```


<br/>

---

## 기억해야 할 내용

책에서 챕터 마지막 부분에 적혀있는 내용입니다.

{{< admonition tip >}}
Beware of functions and methods that iterate over input arguments multiple times. If these arguments are iterators, you may see strange behavior and missing values.  
**입력 인자를 여러 번 이터레이션하는 함수나 메서드를 조심하라. 입력받은 인자가 이터레이터면 함수가 이상하게 작동하거나 결과가 없을 수 있다.**
{{< /admonition >}}

{{< admonition tip >}}
Python's iterator protocol defines how containers and iterators interact with the `iter` and `next` built-in functions, `for` loops, and related expressions.  
**파이썬의 이터레이터 프로토콜은 컨테이너와 이터레이터가 iter, next 내장 함수나 for 루프 등의 관련 식과 상호작용하는 절차를 정의한다.**
{{< /admonition >}}

{{< admonition tip >}}
You can easily define your own iterable container type by implementing the `__iter__` method as a generator.  
**`__iter__` 메서드를 제너레이터로 정의하면 쉽게 이터러블 컨테이너 타입을 정의할 수 있다.**
{{< /admonition >}}

{{< admonition tip >}}
You can detect that a value is an iterator (instead of a container) if calling `iter` on it produces the same value as what you passed in. Alternatively, you can use the `isinstance` built-in function along with the `collections.abc.Iterator` class.  
**어떤 값이 (컨테이너가 아닌) 이터레이터인지 감지하려면, 이 값을 iter 내장 함수에 넘겨서 반환되는 값이 원래 값과 같은지 확인하면 된다. 다른 방법으로 collections.abc.Iterator 클래스를 isinstance와 함께 사용할 수도 있다.**
{{< /admonition >}}

<br/>

---


## lambda 이용해서 필요할 때마다 이터레이터 생성

이터레이터를 필요할 때마다 다시 만드는 방법입니다. 추천하지는 않습니다.

```python
def read_visits(data_path):
    with open(data_path) as f:
        for line in f:
            yield int(line)


# Example 8
def normalize_func(get_iter):
    total = sum(get_iter())   # New iterator
    result = []
    for value in get_iter():  # New iterator
        percent = 100 * value / total
        result.append(percent)
    return result


# Example 9
path = 'my_numbers.txt'
percentages = normalize_func(lambda: read_visits(path))
print(percentages)  # [11.538461538461538, 26.923076923076923, 61.53846153846154]
assert sum(percentages) == 100.0
```

<br/>

---

## 인자가 Iterator 인지 확인

isinstance 로 Iterator 인지 확인할 수 있습니다. 우리가 원하는 건 컨테이너여야 합니다.  
리스트와 이터레이터 프로토콜을 구현한 컨테이너 클래스는 Iterator 가 아닌 걸로 판단되서 문제가 없고, 제너레이터 함수로 만들어진 이터레이터는 조건에 걸려서 에러가 발생합니다.

```python
# Example 13
from collections.abc import Iterator


def normalize_defensive(numbers):
    if isinstance(numbers, Iterator):  # Another way to check
        raise TypeError(f'Must supply a container, {type(numbers)}')
    total = sum(numbers)
    result = []
    for value in numbers:
        percent = 100 * value / total
        result.append(percent)
    return result


# Example 10
class ReadVisits:
    def __init__(self, data_path):
        self.data_path = data_path

    def __iter__(self):
        with open(self.data_path) as f:
            for line in f:
                yield int(line)


# 제너레이터
def read_visits(data_path):
    with open(data_path) as f:
        for line in f:
            yield int(line)


# Example 14
visits = [15, 35, 80]
print(type(visits))  # <class 'list'>
percentages = normalize_defensive(visits)
assert sum(percentages) == 100.0

path = 'my_numbers.txt'
visits = ReadVisits(path)
print(type(visits))  # <class '__main__.ReadVisits'>
percentages = normalize_defensive(visits)
assert sum(percentages) == 100.0

visits = read_visits(path)  # 요건 제너레이터
print(type(visits))  # <class 'generator'>
percentages = normalize_defensive(visits)
# TypeError: Must supply a container, <class 'generator'>
```

<br/>

---

## 참고 자료

- [파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)
- [길벗출판사 깃허브](https://github.com/gilbutITbook/080235/blob/master/Chapter4/Better%20way31.py)
- [원저자 깃허브](https://github.com/bslatkin/effectivepython/blob/master/example_code/item_31.py)

<br/>

---