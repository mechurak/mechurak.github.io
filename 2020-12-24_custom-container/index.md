# 'Better way 43. 커스텀 컨테이너 타입은 collections.abc를 상속하라' 정리


## 0. 들어가며

[Effective Python 2nd 파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)을 제대로 이해하고자 블로그에 정리합니다.

{{< figure src="http://image.kyobobook.co.kr/images/book/xlarge/191/x4801165213191.jpg" width="180px" >}}

<br/>
<br/>

**현재 위치**
{{< admonition note >}}
<5. Classes and Interfaces>  
Item 43: Inherit from `collections.abc` for Custom Container Types  
**Better way 43. 커스텀 컨테이너 타입은 collections.abc를 상속하라**
{{< /admonition >}}


<br/>

---

## 1. 한 줄 요약 및 첨언

파이썬 관례에 따르는 커스텀 컨테이너를 만들어야 한다면, collections.abc 를 활용하는 게 좋습니다.


<br/>

---

## 2. 사용 예시

바이너리 트리를 리스트처럼 쓰고 싶은 상황 예제 입니다.  
책의 예제는 설명을 위해서 여러 클래스로 나뉘어져 있는데, 여기서는 하나로 합쳤습니다.


```python
from collections.abc import Sequence


class BinaryNode(Sequence):  # collections.abc 모듈의 Sequence 상속
    def __init__(self, value, left=None, right=None):
        self.value = value
        self.left = left
        self.right = right

    def _traverse(self):
        if self.left is not None:
            yield from self.left._traverse()
        yield self
        if self.right is not None:
            yield from self.right._traverse()

    def __getitem__(self, index):  # 구현 안하면 컴파일 에러
        for i, item in enumerate(self._traverse()):
            if i == index:
                return item.value
        raise IndexError(f'Index {index} is out of range')

    def __len__(self):  # 구현 안하면 컴파일 에러
        for count, _ in enumerate(self._traverse(), 1):
            pass
        return count


tree = BinaryNode(
    10,
    left=BinaryNode(
        5,
        left=BinaryNode(2),
        right=BinaryNode(
            6,
            right=BinaryNode(7))),
    right=BinaryNode(
        15,
        left=BinaryNode(11))
)

print('LRR is', tree.left.right.right.value)  # LRR is 7
print('Index 0 is', tree[0])  # Index 0 is 2
print('Index 1 is', tree[1])  # Index 1 is 5
print('11 in the tree?', 11 in tree)  # 11 in the tree? True
print('17 in the tree?', 17 in tree)  # 17 in the tree? False
print('Tree is', list(tree))  # Tree is [2, 5, 6, 7, 10, 11, 15]
print('Tree length is', len(tree))  # Tree length is 7
print('Index of 7 is', tree.index(7))  # Index of 7 is 3
print('Count of 10 is', tree.count(10))  # Count of 10 is 1
```

collections.abc 모듈의 클래스를 상속 받으면,  
구현 안된 메서드가 있을 때 컴파일 에러로 알려주고  
일부 메서드는 이미 구현된 걸 사용할 수 있습니다. (Sequence 의 경우는 index, count)

<br/>

---

## 3. 기억해야 할 내용

책에서 챕터 마지막 부분에 적혀있는 내용입니다.

{{< admonition tip >}}
Inherit directly from Python's container types (like `list` or `dict`) for simple use cases.  
간편하게 사용할 경우에는 파이썬 컨테이너 타입(리스트나 딕셔너리 등)을 직접 상속하라.
{{< /admonition >}}

{{< admonition tip >}}
Beware of the large number of methods required to implement custom container types correctly.  
커스텀 컨테이너를 제대로 구현하려면 수많은 메서드를 구현해야 한다는 점에 주의하라.
{{< /admonition >}}

{{< admonition tip >}}
Have your custom container types inherit from the interfaces defined in `collections.abc` to ensure that your classes match required interfaces and behaviors.  
커스텀 컨테이터 타입이 `collection.abc`에 정의된 인터페이스를 상속하면 커스텀 컨테이너 타입이 정상적으로 작동하기 위해 필요한 인터페이스와 기능을 제대로 구현하도록 보장할 수 있다.
{{< /admonition >}}

<br/>

---

## 10. 참고 자료

- [파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)
- [원저자 깃허브](https://github.com/bslatkin/effectivepython/blob/master/example_code/item_43.py)

<br/>

---
