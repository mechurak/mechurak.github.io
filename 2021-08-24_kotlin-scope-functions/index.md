# Kotlin에서의 Scope functions (let, run, with, apply, also)


## 0. 한 줄 요약

- 특별한 기능이 있는 것은 아니고, 불필요한 변수 선언을 없애고 코드의 가독성을 높이기 위함
- 비슷한 기능을 하므로, 팀 혹은 프로젝트 단위로 일관된 convention 을 가지고 사용해야 함
- 막 쓰지 말고, 중첩해서 쓰지 말자

<br/>

---

## 1. Scope functions?

[공식 문서](https://kotlinlang.org/docs/scope-functions.html)의 설명은 아래와 같습니다.

> The Kotlin standard library contains several functions whose sole purpose is to execute a block of code within the context of an object. When you call such a function on an object with a [lambda expression](https://kotlinlang.org/docs/lambdas.html) provided, it forms a temporary scope. In this scope, you can access the object without its name. Such functions are called scope functions. There are five of them: let, run, with, apply, and also.
> 
> Basically, these functions do the same: execute a block of code on an object. What's different is how this object becomes available inside the block and what is the result of the whole expression.

제한된 scope 를 만들고, 고 안에서 관심있는 object를 **이름 없이** 갖다 쓰는게 주 목적인 함수라고 생각하면 될 것 같습니다.  
아마도 쓸 데없는 변수 선언을 없애서 사이드 이펙트를 줄이고, 해당 코드 블럭을 하나의 expression 으로 간주하기 위함으로 보입니다.  

일단 코드를 봅시다.

```kotlin
// let 사용
Person("Alice", 20, "Amsterdam").let {
    println(it)
    it.moveTo("London")
    it.incrementAge()
    println(it)
}
```

```kotlin
// let 사용하지 않는 경우
val alice = Person("Alice", 20, "Amsterdam")
println(alice)
alice.moveTo("London")
alice.incrementAge()
println(alice)
```

`let` 의 예시 입니다.  
`let`을 사용하지 않은 코드와 비교해보면,  
`alice` 라는 추가 변수를 사용하지 않았고,  
블럭 내부에서 `it`로 Person 객체에 접근하고 있습니다.

<br/>

---

## 2. 다섯 함수들의 차이 점

큰 틀에서 목적은 같아서, 다섯 함수들 모두 동작은 비슷하지만,  
- context object 를 어떻게 접근하는지
- 어떤 값을 리턴하는지
- [extension function](https://kotlinlang.org/docs/extensions.html#extension-functions) 인지 그냥 function 인지

가 차이점입니다.

| Function | Object reference | Return value   | Is extension function | 비고    |
| :------: | :--------------: | :------------: | --------------------- | ------- |
| `let`    | `it`             | Lambda result  | Yes                   | Executing a lambda on non-null objects. Introducing an expression as a variable in local scope |
| `run`    | `this`           | Lambda result  | Yes                   | Object configuration and computing the result |
| `run`    | -                | Lambda result  | No: called without the context object | Running statements where an expression is required |
| `with`   | `this`           | Lambda result  | No: takes context object as an argument | with this object, do the following |
| `apply`  | `this`           | Context object | Yes                   | apply the following assignments to the object |
| `also`   | `it`             | Context object | Yes                   | and also do the following with the object |

`this` 로 접근할 수 있다는 것은, 해당 object 의 method나 variable에 이름 없이 바로 접근이 가능하다는 것을 의미합니다.  
Context object 를 리턴한다는 것은, 자기 자신을 리턴하므로 builder 패턴 처럼 사용 가능합니다.

각 함수들의 예시는 "4. Reference" 의 블로그들에 정리가 잘 되어 있습니다. 한 번 이해하고 나면 나중에 헷갈릴 때 [위의 표 (원문 링크)](https://kotlinlang.org/docs/scope-functions.html#function-selection)를 참고하면 될 것 같습니다.

<br/>

---

## 3. 권장되는 사용처

[공식 문서](https://kotlinlang.org/docs/scope-functions.html#function-selection)에서 가이드하는 적절한 사용처는 아래와 같습니다.

- Executing a lambda on non-null objects: `let`
- Introducing an expression as a variable in local scope: `let`
- Object configuration and computing the result: `run`
- Running statements where an expression is required: non-extension `run`
- Grouping function calls on an object: `with`
- Object configuration: `apply`
- Additional effects: `also`

<br/>

---

## 4. Reference

- [코틀린 let, with, run, apply, also 차이 비교 정리 / 블로그](https://blog.yena.io/studynote/2020/04/15/Kotlin-Scope-Functions.html)
- [코틀린의 Scope Functions(let, with, run, apply, also)을 알아보자 / 블로그](https://developer88.tistory.com/180)
- [let을 null check으로 쓰지 마세요 / 블로그](https://tourspace.tistory.com/208?category=797357)
- [Scope functions / 코틀린 공식 문서](https://kotlinlang.org/docs/scope-functions.html)

<br/>

---

