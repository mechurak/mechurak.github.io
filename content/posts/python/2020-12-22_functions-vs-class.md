---
title: "'Better way 38. 간단한 인터페이스의 경우 클래스 대신 함수를 받아라' 정리"
date: 2020-12-22T01:01:00+09:00
summary: "간단한 인터페이스가 필요할 때는 클래스를 정의하고 인스턴스화하는 대신 간단히 함수를 사용할 수 있습니다."
categories: [ python ]
series: [ effective python 5. 클래스와 인터페이스 ]
series_weight: 38
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
Item 38: Accept Functions Instead of Classes for Simple Interfaces  
**Better way 38. 간단한 인터페이스의 경우 클래스 대신 함수를 받아라**
{{< /admonition >}}


<br/>

---

## 1. 한 줄 요약 및 첨언

간단한 인터페이스가 필요할 때는 클래스를 정의하고 인스턴스화하는 대신 간단히 함수를 사용할 수 있습니다.



<br/>

---

## 2. 사용 예시

defaultdict 은, 특정 상황(없는 키로 접근)에서 호출할 함수를 넘겨 받습니다.

```python
from collections import defaultdict


# Example 2
def log_missing():
    print('Key added')
    return 0


# Example 3
current = {'green': 12, 'blue': 3}

# 특정상황에서 넘겨 준 함수(log_missing)을 실행해 줌
# 인자로 받는 함수를 훅(hook)이라고 함.
result = defaultdict(log_missing, current)
print('Before:', dict(result))

increments = [
    ('red', 5),
    ('blue', 17),
    ('orange', 9),
]
for key, amount in increments:
    result[key] += amount
print('After: ', dict(result))
```

```실행결과.txt
Before: {'green': 12, 'blue': 3}
Key added
Key added
After:  {'green': 12, 'blue': 20, 'red': 5, 'orange': 9}
```

<br/>

이제 존재하지 않는 키에 접근한 횟수를 세고 싶은 상황입니다.  
호출 가능(callable) 객체를 사용한 예제입니다. 저 아래 클로저를 사용하는 것보다 명확합니다.

```python
from collections import defaultdict


# Example 8
class BetterCountMissing:
    def __init__(self):
        self.added = 0

    def __call__(self):  # 호출 가능 객체
        self.added += 1
        return 0


current = {'green': 12, 'blue': 3}
increments = [
    ('red', 5),
    ('blue', 17),
    ('orange', 9),
]


# Example 9
counter = BetterCountMissing()
result = defaultdict(counter, current)  # Relies on __call__
for key, amount in increments:
    result[key] += amount

print(counter.added)  # 2
print(dict(result))  # {'green': 12, 'blue': 20, 'red': 5, 'orange': 9}
```

<br/>

---

## 3. 기억해야 할 내용

책에서 챕터 마지막 부분에 적혀있는 내용입니다.

{{< admonition tip >}}
Instead of defining and instantiating classes, you can often simple use functions for simple interfaces between components in Python.  
**파이썬의 여러 컴포넌트 사이에 간단한 인터페이스가 필요할 때는 클래스를 정의하고 인스턴스화하는 대신 간단히 함수를 사용할 수 있다.**
{{< /admonition >}}

{{< admonition tip >}}
References to functions and methods in Python are first class, meaning they can be used in expressions (like any other type).  
**파이썬 함수나 메서드는 일급 시민이다. 따라서 (다른 타입의 값과 마찬가지로) 함수나 함수 참조를 식에 사용할 수 있다.**
{{< /admonition >}}

{{< admonition tip >}}
The `__call__` special method enables instances of a class to be called like plain Python functions.  
**`__call__` 특별 메서드를 사용하면 클래스의 인스턴스 객체를 일반 파이썬 함수처럼 호출할 수 있다.**
{{< /admonition >}}

{{< admonition tip >}}
When you need a function to maintain state, consider defining a class that provides the `__call__` method instead of defining a stateful closure.  
**상태를 유지하기 위한 함수가 필요한 경우에는 상태가 있는 클로저를 정의하는 대신 `__call__` 메서드가 있는 클래스를 정의할지 고려해보라.**
{{< /admonition >}}

<br/>

---


## 4. 더 알아보기

위와 같이 존재하지 않는 키에 접근한 횟수를 세고 싶은 상황입니다.

### 4.1 상태가 있는 클로저 사용

Stateful closure를 사용한 예제입니다.  
복잡하니 사용하지 맙시다.

```python
from collections import defaultdict


# Example 4
def increment_with_report(current, increments):
    added_count = 0

    def missing():
        nonlocal added_count  # Stateful closure
        added_count += 1
        return 0

    result = defaultdict(missing, current)
    for key, amount in increments:
        result[key] += amount

    return result, added_count


current = {'green': 12, 'blue': 3}
increments = [
    ('red', 5),
    ('blue', 17),
    ('orange', 9),
]

# Example 5
result, count = increment_with_report(current, increments)
print(count)  # 2
print(dict(result))  # {'green': 12, 'blue': 20, 'red': 5, 'orange': 9}
```

### 4.2 메서드 사용

추적하고 싶은 상태를 저장하는 작은 클래스를 정의하였습니다.

```python
from collections import defaultdict


# Example 6
class CountMissing:
    def __init__(self):
        self.added = 0

    def missing(self):
        self.added += 1
        return 0


current = {'green': 12, 'blue': 3}
increments = [
    ('red', 5),
    ('blue', 17),
    ('orange', 9),
]

# Example 7
counter = CountMissing()
result = defaultdict(counter.missing, current)  # Method ref
for key, amount in increments:
    result[key] += amount
print(counter.added)  # 2
print(dict(result))  # {'green': 12, 'blue': 20, 'red': 5, 'orange': 9}
```

바로 위 Stateful closure를 사용하는 것보다 깔끔합니다.  
하지만 CountMissing 클래스의 목적이 명확하게 보이지 않습니다. 누가 CountMissing 객체를 만들지, 누가 missing 메서드를 호출할지, 공개 메서드가 더 추가될 수 있을지, 사용 예제를 보기 전까지는 수수께끼일 뿐입니다.

<br/>

---

## 10. 참고 자료

- [파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)
- [원저자 깃허브](https://github.com/bslatkin/effectivepython/blob/master/example_code/item_38.py)

<br/>

---