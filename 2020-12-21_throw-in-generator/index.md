# 'Better way 35. 제너리이터 안에서 throw로 상태를 변화시키지 말라' 정리


## 0. 들어가며

[Effective Python 2nd 파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)을 제대로 이해하고자 블로그에 정리합니다.

{{< figure src="http://image.kyobobook.co.kr/images/book/xlarge/191/x4801165213191.jpg" width="180px" >}}

<br/>
<br/>

**현재 위치**
{{< admonition note >}}
<4. Comprehensions and Generators>  
Item 35: Avoid Causing State Transitions in Generators with `throw`  
**Better way 35. 제너리이터 안에서 throw로 상태를 변화시키지 말라**
{{< /admonition >}}


<br/>

---

## 1. 한 줄 요약 및 첨언

제너레이터에 throw 를 호출하지 말고, `__iter__` 를 구현한 클래스를 이용해서 상태를 바꾸는 게 좋다고 합니다.

요런 것까지 쓸 일이 있나 싶긴 합니다.

<br/>

---

## 2. 사용 예시

```python
RESETS = [
    False, False, True, False, True,
    False, False, False, False, False]


def check_for_reset():
    # Poll for external event
    return RESETS.pop(0)  # 앞에꺼 하나씩 (2, 4번 인덱스 True)


def announce(remaining):
    print(f'{remaining} ticks remaining')


# Example 5
class Timer:
    def __init__(self, period):
        self.current = period
        self.period = period

    def reset(self):
        print("reset!!", end=' ')
        self.current = self.period

    def __iter__(self):
        while self.current:
            self.current -= 1
            yield self.current


def run():
    timer = Timer(4)
    for i, current in enumerate(timer):  # __iter__() 사용해서 for 문 수행됨
        print(f'[{i}] current({current})', end=': ')
        if check_for_reset():  # 외부 이벤트 확인
            timer.reset()  # 다시 처음(4) 부터
        announce(current)


run()
```

실행 결과

```txt
[0] current(3): 3 ticks remaining
[1] current(2): 2 ticks remaining
[2] current(1): reset!! 1 ticks remaining
[3] current(3): 3 ticks remaining
[4] current(2): reset!! 2 ticks remaining
[5] current(3): 3 ticks remaining
[6] current(2): 2 ticks remaining
[7] current(1): 1 ticks remaining
[8] current(0): 0 ticks remaining
```

<br/>

---

## 3. 기억해야 할 내용

책에서 챕터 마지막 부분에 적혀있는 내용입니다.

{{< admonition tip >}}
The `throw` method can be used to re-raise exceptions within generators at the position of the most recently executed `yield` expression.  
**`throw` 메서드를 사용하면 제너레이터가 마지막으로 실행한 `yield` 식의 위치에서 예외를 다시 발생시킬 수 있다.**
{{< /admonition >}}

{{< admonition tip >}}
Using `throw` harms readability because it requires additional nesting and boilerplate in order to raise and catch exceptions.  
**`throw`를 사용하면 가독성이 나빠진다. 예외를 잡아내고 다시 발생시키는 데 준비 코드가 필요하며 내포 단계가 깊어지기 때문이다.**
{{< /admonition >}}

{{< admonition tip >}}
A better way to provide exceptional behavior in generators is to use a class that implements the `__iter__` method along with methods to cause exceptional state transitions.  
**제너레이터에서 예외적인 동작을 제공하는 더 나은 방법은 `__iter__` 메서드를 구현하는 클래스를 사용하면서 예외적인 경우에 상태를 전이시키는 것이다.**
{{< /admonition >}}

<br/>

---


## 4. 더 알아보기

### 4.1 제너레이터에 throw 호출

제너레이터 안에서 마지막으로 실행된 yield 문을 둘러쌈으로써 예외를 잡아낼 수 있습니다.

```python
class MyError(Exception):
    pass


# Example 2
def my_generator():
    yield 1

    try:
        yield 2  # 여기까지 진행된 상태
    except MyError:
        print('Got MyError!')  # throw 로 받았음
    else:
        yield 3  # except 들어 갔으니 요건 패스

    yield 4  # except 처리하고 요거 리턴


it = my_generator()
print(next(it))  # 1
print(next(it))  # 2
print(it.throw(MyError('test error')))  # 4 
```

실행 결과

```txt
1
2
Got MyError!
4
```

### 4.2 throw 로 위의 Timer 와 같은 동작을 한다면

제너레이터 안에서 exception 을 캐치하면 바로 수정된 값을 던져주는 게 다르지만, 비슷한 컨셉으로 동작합니다.

어쨋든 사용하지 맙시다.

```python
# Example 3
class Reset(Exception):
    pass


def timer(period):
    current = period
    while current:
        current -= 1
        try:
            yield current
        except Reset:
            print('Reset!!', end=' ')
            current = period


RESETS = [
    False, False, True, False, True,
    False, False, False, False, False]


def check_for_reset():
    # Poll for external event
    return RESETS.pop(0)  # 앞에꺼 하나씩 (3, 5번 인덱스 True)


def announce(remaining):
    print(f'{remaining} ticks remaining')


def run():
    it = timer(4)
    i = 0
    while True:
        print(f'[{i}]', end=' ')
        try:
            if check_for_reset():
                current = it.throw(Reset())  # throw 하면, 바로 3을 받음
            else:
                current = next(it)
        except StopIteration:
            break
        else:
            announce(current)
        i += 1


run()
```

실행 결과

```txt
[0] 3 ticks remaining
[1] 2 ticks remaining
[2] Reset!! 3 ticks remaining
[3] 2 ticks remaining
[4] Reset!! 3 ticks remaining
[5] 2 ticks remaining
[6] 1 ticks remaining
[7] 0 ticks remaining
[8] 
```

<br/>

---

## 10. 참고 자료

- [파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)
- [원저자 깃허브](https://github.com/bslatkin/effectivepython/blob/master/example_code/item_35.py)
- ['Better way 31. 인자에 대해 이터레이션 할 때는 방어적이 되라' 정리](https://mechurak.github.io/ko/posts/python/2020-12-18_iterating-over-argument/)

<br/>

---
