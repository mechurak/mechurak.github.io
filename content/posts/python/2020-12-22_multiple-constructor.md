---
title: "'Better way 39. 객체를 제너릭하게 구성하려면 @classmethod를 통한 다형성을 활용하라' 정리"
date: 2020-12-22T01:02:00+09:00
summary: "`@classmethod` 를 사용해서 하위 클래스를 같은 방식으로 사용할 수 있습니다."
categories: [ python ]
series: [ effective python 5. 클래스와 인터페이스 ]
series_weight: 39
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
Item 39. Use `@classmethod` Polymorphism to Construct Objects Generically  
**Better way 39. 객체를 제너릭하게 구성하려면 `@classmethod`를 통한 다형성을 활용하라**
{{< /admonition >}}


<br/>

---

## 1. 한 줄 요약 및 첨언

`@classmethod` 를 사용해서 하위 클래스를 같은 방식으로 사용할 수 있습니다.



<br/>

---

## 2. 사용 예시

책의 예제는 너무 복잡해서 조금 줄여 보았습니다.

```python
class GenericInputData:
    def read(self):
        raise NotImplementedError

    @classmethod  # 클래스메서드로 표시
    def generate_inputs(cls, config):
        raise NotImplementedError


class PathInputData(GenericInputData):
    def __init__(self, i):
        super().__init__()
        self.path = f'temp_dir/{i}'

    def read(self):
        return self.path

    @classmethod
    def generate_inputs(cls, start):  # 클래스를 인자로 받음
        for i in range(start, start + 5):
            yield cls(i)  # 인자로 받은 클래스로 인스턴스 생성


# 부모 클래스(GenericInput) 로 코딩
def temp_generate_inputs(input_class, start):
    it = input_class.generate_inputs(start)
    return it

# 최초 사용 부분에서 자식 클래스 지정(PathInputData)
data = temp_generate_inputs(PathInputData, 3)
for x in data:  # x는 PathInputData 객체
    print(x.read())
```


```실행결과.txt
temp_dir/3
temp_dir/4
temp_dir/5
temp_dir/6
temp_dir/7
```

<br/>

---

## 3. 기억해야 할 내용

책에서 챕터 마지막 부분에 적혀있는 내용입니다.

{{< admonition tip >}}
Python only supports a single constructor per class: the `__init__` method.  
**파이썬의 클래스에는 생성자가 `__init__` 메서드 뿐이다.**
{{< /admonition >}}

{{< admonition tip >}}
Use `@classmethod` to define alternative constructors for your classes.  
**`@classmethod`를 사용하면 클래스에 다른 생성자를 정의할 수 있다.**
{{< /admonition >}}

{{< admonition tip >}}
Use class method polymorphism to provide generic ways to build and connect many concrete subclasses.  
**클래스 메서드 다형성을 활용하면 여러 구체적인 하위 클래스의 객체를 만들고 연결하는 제너릭한 방법을 제공할 수 있다.**
{{< /admonition >}}

<br/>

---

## 10. 참고 자료

- [파이썬 코딩의 기술 (교보문고 링크)](http://digital.kyobobook.co.kr/digital/ebook/ebookDetail.ink?selectedLargeCategory=001&barcode=4801165213191&orderClick=LEH&Kc=)
- [원저자 깃허브](https://github.com/bslatkin/effectivepython/blob/master/example_code/item_39.py)

<br/>

---