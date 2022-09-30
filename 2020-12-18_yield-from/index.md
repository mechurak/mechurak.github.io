# 'Better way 33. yield from을 사용해 여러 제너레이터를 합성하라' 정리


## 들어가며

[Effective Python 2nd 파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)을 제대로 이해하고자 블로그에 정리합니다.

{{< figure src="http://image.kyobobook.co.kr/images/book/xlarge/191/x4801165213191.jpg" width="180px" >}}

<br/>
<br/>

**현재 위치**
{{< admonition note >}}
<4. Comprehensions and Generators>  
Item 33: Compose Multiple Generators with `yield from`  
**Better way 33. yield from을 사용해 여러 제너레이터를 합성하라**
{{< /admonition >}}


<br/>

---

## 한 줄 요약 및 첨언

제너레이터가 제너레이터를 내포할 때는 `yield from`으로 내포된 제네레이터에서 값을 바로 내보냅시다.

<br/>

---

## 사용 예시

제너레이터를 사용해 한 루프마다 이미지의 변위를 만들어 내는 동작이 필요한 상황입니다.

```python
def move(period, speed):
    for _ in range(period):
        yield speed


def pause(delay):
    for _ in range(delay):
        yield 0


def render(delta):
    print(f'Delta: {delta:.1f}')
    # Move the images onscreen


def run(func):
    for delta in func():
        render(delta)


# Example 4 (yield from 사용)
def animate_composed():
    yield from move(4, 5.0)
    yield from pause(3)
    yield from move(2, 3.0)


run(animate_composed)
```

```
Delta: 5.0
Delta: 5.0
Delta: 5.0
Delta: 5.0
Delta: 0.0
Delta: 0.0
Delta: 0.0
Delta: 3.0
Delta: 3.0
```

<br/>

---

## 기억해야 할 내용

책에서 챕터 마지막 부분에 적혀있는 내용입니다.

{{< admonition tip >}}
The `yield from` expression allows you to compose multiple nested generators together into a single combined generator.  
**`yield from` 식을 사용하면 여러 내장 제너레이터를 모아서 제너레이터 하나로 합성할 수 있다.**
{{< /admonition >}}

{{< admonition tip >}}
`yield from` provides better performance than manually iterating nested generators and yielding their outputs.  
**직접 내포된 제너레이터를 이터레이션하면서 각 제너레이터의 출력을 내보내는 것보다 yield from을 사용하는 것이 성능 면에서 더 좋다.**
{{< /admonition >}}

<br/>

---


## yield from 성능 비교

tileit 내장 모듈을 사용한 벤치마크

```python
# Example 5
import timeit


def child():
    for i in range(1_000_000):
        yield i


# yield from 사용하지 않은 구현
def slow():
    for i in child():
        yield i


# yield from 사용한 구현
def fast():
    yield from child()


baseline = timeit.timeit(
    stmt='for _ in slow(): pass',
    globals=globals(),
    number=50)
print(f'Manual nesting {baseline:.2f}s')

comparison = timeit.timeit(
    stmt='for _ in fast(): pass',
    globals=globals(),
    number=50)
print(f'Composed nesting {comparison:.2f}s')

reduction = -(comparison - baseline) / baseline
print(f'{reduction:.1%} less time')
```

```
Manual nesting 6.00s
Composed nesting 5.11s
14.8% less time
```

<br/>

---

## 참고 자료

- [파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)
- [길벗출판사 깃허브](https://github.com/gilbutITbook/080235/blob/master/Chapter4/Better%20way33.py)
- [원저자 깃허브](https://github.com/bslatkin/effectivepython/blob/master/example_code/item_33.py)

<br/>

---
