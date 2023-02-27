# Dart, Flutter 에서의 try catch


Dart에서 try-catch 를 사용해서 exception을 처리해보자.
<!--more-->

{{< image src="/images/logo/flutter.png" caption="Flutter">}}

<br/>
<br/>

## 핵심요약

{{< admonition >}}
- `try-on-catch` 구문으로 expeciton 처리
- `on` 으로 catch 할 타입 지정
- `throw` 로 expection 
- Dart는 기본적으로 error 핸들링을 강요하지는 않음 (Unchecked excpetion)
{{< /admonition >}}

<br/>

---

## 사용 방법
### custom exception 정의
```dart
class MyException implements Exception {
  final String? msg;

  const MyException([this.msg]); // []: optional positional parameters

  @override
  String toString() => msg ?? 'MyException';
  // ?? : conditional expressions. 앞에꺼가 null 아니면 고거 리턴, 아니면 뒤에꺼
}
```
본인 프로젝트에 맞게 적절하게 이름도 짓고 변수 및 생성자도 추가하면 되겠다.

<br/>

### 정의한 exception을 throw
```dart {hl_lines=[7]}
Future<void> foo() async {
  print("  2.1. start foo()");
  print("  2.2. trigger a long task...");
  await Future.delayed(Duration(seconds: 1));  
  if (true) {  // 특정 조건에서
    print("  2.3. throw exception!!");
    throw MyException("My Awesome Exception");  // exception 발생시킴
  }
  print("  2.4. another long task...");
  await Future.delayed(Duration(seconds: 1));
  print("  2.5. task has done");
}
```
비동기 함수 내부에서 exception 이 발생하는 상황을 따져 보고 싶었다.  
함수 수행 도중에 의도치 않은 상황이 발생하면, `throw` 로 exception 을 발생시켜서, 호출부에서 에러를 보여준다든지 추가 처리를 하는 상황이 될 것 같다.


<br/>

### try-on-catch
```dart {hl_lines=[3,7]}
main() async {
  print("1. main() started.");
  try {
    print("2. Before foo()");
    await foo();
    print("3. After foo()");
  } on MyException catch (e) {
    print("2e. Caught $e");
  }
  print("4. main() ended.");
}
```
`on` 으로 catch 할 타입을 명시해주고, `catch (e)` 로 블럭 내에서 `e`를 참조할 수 있도록 한다.  


<br/>

### 실행 결과
```
1. main() started.
2. Before foo()
  2.1. start foo()
  2.2. trigger a long task...
  2.3. throw exception!!
2e. Caught My Awesome Exception
4. main() ended.
```
foo() 함수에서 `2.3` 출력 이후에 `throw` 가 불려셔, `2.4`, `2.5` 부분은 실행되지 않았다.
main() 함수에서 `3. After foo()`는 보이지 않는다.

<br/>

### DartPad에서 확인
적당히 바꿔서 테스트도 해보자.

<iframe style="width:100%;height:1000px;" src="https://dartpad.dev/embed-inline.html?id=9b729fe1ef44d65e467fc5894356f063&split=75&theme=dark&run=true"></iframe>

<br/>

---

## 더 알아보기

### Dart에서는 모든 Exception이 UncheckedException

Java에서 `Exception`이랑 `Error`가 뭐가 달랐었는지, `CheckedException` 이랑 `UncheckedException`이 뭐였는지 살펴보자. [{Tistory 블로그} [Java] Error와 Exception](https://choiblack.tistory.com/39) 에 잘 설명되어 있다.

{{< image src="https://madplay.github.io/img/post/2019-03-02-java-checked-unchecked-exceptions-1.png" caption="이미지 출처: [{블로그} 자바 예외 구분: Checked Exception, Unchecked Exception](https://madplay.github.io/post/java-checked-unchecked-exceptions)" >}}

[{Dart 공식문서} A tour of the Dart language - Exceptions](https://dart.dev/guides/language/language-tour#exceptions) 은 아래와 같이 설명하고 있다.

> In contrast to Java, all of Dart’s exceptions are unchecked exceptions. Methods don’t declare which exceptions they might throw, and you aren’t required to catch any exceptions.



<br/>

---


## Reference
- [{Stack Overflow} How do I return error from a Future in dart?](https://stackoverflow.com/a/57736915/16111308)
- [{Dart 공식문서} A tour of the Dart language - Exceptions](https://dart.dev/guides/language/language-tour#exceptions)
- [{Dart 공식문서} A tour of the core libraries - Exceptions](https://dart.dev/guides/libraries/library-tour#exceptions)
- [{Tistory 블로그} [Java] Error와 Exception](https://choiblack.tistory.com/39)

<br/>

---
