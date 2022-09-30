---
title: "'Better way 34. send로 제너레이터에 데이터를 주입하지 말라' 정리"
date: 2020-12-21T01:00:00+09:00
summary: "제너레이터에 `send`를 통해 데이터를 주입하는게 가능하기는 한데, 안쓰는 게 좋다고 합니다."
categories: [ python ]
series: [ effective python 4. 컴프리헨션과 제너레이터 ]
series_weight: 34
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
<4. Comprehensions and Generators>  
Item 34: Avoid Injecting Data into Generators with `send`  
**Better way 34. send로 제너레이터에 데이터를 주입하지 말라**
{{< /admonition >}}


<br/>

---

## 1. 한 줄 요약 및 첨언

제너레이터에 `send`를 통해 데이터를 주입하는게 가능하기는 합니다.  
하지만 제너레이터는 그렇게 안쓰는 게 좋다고 합니다.

원래 몰랐으니, 계속 모르도록 하겠습니다.

<br/>

---

## 2. 사용 예시

N/A

<br/>

---

## 3. 기억해야 할 내용

책에서 챕터 마지막 부분에 적혀있는 내용입니다.

{{< admonition tip >}}
The `send` method can be used to inject data into a generator by giving the `yield` expression a value that can be assigned to a variable.  
**send 메서드를 사용해 데이터를 제너레이터에 주입할 수 있다. 제너레이터는 send로 주입된 값을 yield 식이 반환하는 값을 통해 받으며, 이 값을 변수에 저장해 활용할 수 있다.**
{{< /admonition >}}

{{< admonition tip >}}
Using `send` with `yield from` expressions may cause surprising behavior, such as `None` values appearing at unexpected times in the generator output.  
**`send`와 `yield from` 식을 함께 사용하면 제너레이터의 출력에 None이 불쑥불쑥 나타나는 의외의 결과를 얻을 수도 있다.**
{{< /admonition >}}

{{< admonition tip >}}
Providing an input iterator to a set of composed generators is a better approach than using the `send` method, which should be avoided.  
**합성할 제너레이터들의 입력으로 이터레이터를 전달하는 방식이 `send`를 사용하는 방식보다 더 낫다. `send`는 가급적 사용하지 말라.**
{{< /admonition >}}

<br/>

---


## 4. 더 알아보기

### 4.1 제너레이터에 send를 꼭 사용한다면

```python
import math


# Example 2 (output 사용처)
def transmit(output):
    if output is None:
        print(f'Output is None')
    else:
        print(f'Output: {output:>5.1f}')


# Example 5 (제너레이터)
def wave_modulating(steps):
    step_size = 2 * math.pi / steps
    amplitude = yield  # Receive initial amplitude (최초 None 을 send 하면 여기까지 진행)
    print(' received ', amplitude)  # 두번째 7 send 하면, 위 라인에서 받아서 다음 yield 까지 진행
    for step in range(steps):
        radians = step * step_size
        fraction = math.sin(radians)
        output = amplitude * fraction
        print(' prepared output', amplitude, output)
        amplitude = yield output  # Receive next amplitude
        print(' received ', amplitude)


# Example 6
def run_modulating(it):
    # 시작 안된 제너레이터에는 None 만 보낼 수 있음
    amplitudes = [None, 7, 7, 7, 2, 2, 2, 2, 10, 10, 10, 10, 10]
    for i, amplitude in enumerate(amplitudes):
        print(f'\nbefore send. amplitude[{i}]: {amplitude}')
        output = it.send(amplitude)
        print(f'after send. output: {output}')
        transmit(output)


run_modulating(wave_modulating(12))
```

```실행결과.txt
before send. amplitude[0]: None
after send. output: None
Output is None

before send. amplitude[1]: 7
 received  7
 prepared output 7 0.0
after send. output: 0.0
Output:   0.0

...

before send. amplitude[3]: 7
 received  7
 prepared output 7 6.06217782649107
after send. output: 6.06217782649107
Output:   6.1

before send. amplitude[4]: 2
 received  2
 prepared output 2 2.0
after send. output: 2.0
Output:   2.0
```

<br/>

---

## 10. 참고 자료

- [파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)
- [원저자 깃허브](https://github.com/bslatkin/effectivepython/blob/master/example_code/item_34.py)

<br/>

---