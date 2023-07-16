# Unity C#: String Interpolation


변수 내용들에 대한 로그를 찍거나 String을 만들 때 `$`을 써서 String Interploation 을 사용하자.
<!--more-->

<br/>

{{< image src="/images/logo/unity.png" caption="Unity 로고" width=50% >}}

<br/>

## 핵심요약
{{< admonition >}}
- 따옴표 앞에 `$`를 붙이고, 따옴표 안에서 `{}` 안에 expression
- `String.Format()` 말고 요걸(**String Interpolation**) 쓰자.
{{< /admonition >}}

<br/>

---

## 사용 방법
String이 필요한 부분에서 `"` 앞에 `$`를 붙이고, 따옴표 안에서 `{}` 안에 expression(변수 or 계산 되어서 값으로 나올 수 있는 표현식)을 쓴다.  
- `,-6`(왼쪽 정렬)이나 `,6`(오른쪽 정렬) 요런식으로 정렬 명시 가능
- `:F2`(소수점 아래 두 글자 까지만 표기) 요런 식으로 포매팅도 가능

```cs
  float randomValue1 = Random.value;  // 0.0f ~ 1.0f
  float randomValue2 = Random.value;  // 0.0f ~ 1.0f
  Debug.Log($"1.randomValue1: _{randomValue1}_");  // (1) 변수만
  Debug.Log($"2.randomValue1: _{randomValue1:F2}_");  // (2) 포매팅. 소수점 아래 두 번째 까지
  Debug.Log($"3.randomValue1: _{randomValue1,-6:F2}_");  // (3) 6칸 차지. 왼쪽에 맞춤
  Debug.Log($"4.randomValue1: _{randomValue1,6:F2}_");  // (4) 6칸 차지. 오른쪽에 맞춤
  Debug.Log($"5.randomValue2: _{randomValue2,6:F2}_");  // (5) 6칸 차지. 오른쪽에 맞춤
```

```
1.randomValue1: _0.27493_     # (1) 변수만
2.randomValue1: _0.27_        # (2) 포매팅.  소수점 아래 두 번째 까지
3.randomValue1: _0.27  _      # (3) 6칸 차지. 왼쪽에 맞춤
4.randomValue1: _  0.27_      # (4) 6칸 차지. 오른쪽에 맞춤
5.randomValue2: _  0.39_      # (5) 6칸 차지. 오른쪽에 맞춤
```

<br/>

포매팅은 소수점 자리수 말고는 딱히 쓸 일 없을 것 같은데, 필요해지면 아래 가이드 페이지 및 그 주변 가이드 페지이들을 참고하자.  
- [{Microsoft Learn} Standard numeric format strings](https://learn.microsoft.com/en-us/dotnet/standard/base-types/standard-numeric-format-strings)

<br/>

---

## String.Format() vs String Interpolation
Microsoft Learn의 String.Format() 문서의 [Q&A](https://learn.microsoft.com/en-us/dotnet/api/system.string.format?view=netstandard-2.0#why-do-you-recommend-string-interpolation-over-calls-to-the-stringformat-method) 부분에 아래 내용이 있다.

> **Why do you recommend string interpolation over calls to the String.Format method?**
>
> String interpolation is:
> - **More flexible.** It can be used in any string without requiring a call to a method that supports composite formatting. Otherwise, you have to call the Format method or another method that supports composite formatting, such as `Console.WriteLine` or `StringBuilder.AppendFormat`.
> - **More readable.** Because the expression to insert into a string appears in the interpolated expression rather than in a argument list, interpolated strings are far easier to code and to read. Because of their greater readability, interpolated strings can replace not only calls to composite format methods, but they can also be used in string concatenation operations to produce more concise, clearer code.

String Interpolation이 좋단다. 요걸 주로 쓰고 `String.Format`은 알아만 놓자.

<br/>

- 위 페이지의 예시 코드 `String.Format()`
```cs
  string[] names = { "Balto", "Vanya", "Dakota", "Samuel", "Koani", "Yiska", "Yuma" };
  string output = names[0] + ", " + names[1] + ", " + names[2] + ", " + 
                  names[3] + ", " + names[4] + ", " + names[5] + ", " + 
                  names[6];  

  output += "\n";  
  var date = DateTime.Now;
  output += String.Format("It is {0:t} on {0:d}. The day of the week is {1}.", 
                          date, date.DayOfWeek);
  Console.WriteLine(output);                           
  // The example displays the following output:
  //     Balto, Vanya, Dakota, Samuel, Koani, Yiska, Yuma
  //     It is 10:29 AM on 1/8/2018. The day of the week is Monday.
```

<br/>

- String Interpolation 사용해서 같은 결과
```cs
  string[] names = { "Balto", "Vanya", "Dakota", "Samuel", "Koani", "Yiska", "Yuma" };
  string output = $"{names[0]}, {names[1]}, {names[2]}, {names[3]}, {names[4]}, " + 
                  $"{names[5]}, {names[6]}";  

  var date = DateTime.Now;
  output += $"\nIt is {date:t} on {date:d}. The day of the week is {date.DayOfWeek}.";
  Console.WriteLine(output);                           
  // The example displays the following output:
  //     Balto, Vanya, Dakota, Samuel, Koani, Yiska, Yuma
  //     It is 10:29 AM on 1/8/2018. The day of the week is Monday.
```

<br/>

---

## Reference
- [{Microsoft Learn} String interpolation using `$`](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/tokens/interpolated)
- [{Microsoft Learn} Standard numeric format strings](https://learn.microsoft.com/en-us/dotnet/standard/base-types/standard-numeric-format-strings)
- [{Microsoft Learn} String.Format Method](https://learn.microsoft.com/en-us/dotnet/api/system.string.format?view=netstandard-2.0)

<br/>

---
