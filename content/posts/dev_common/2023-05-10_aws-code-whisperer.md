---
title: "AWS CodeWhisperer 맛보기 (feat. PyCharm)"
date: 2023-05-10T23:00:00+09:00
categories: [ 개발 일반 ]
tags:
- code_whisperer
- jetbrains
- for_free
---

AI가 코드 작성을 도와준단다. GitHub Copilot은 돈 내야 하는데 요건 개인 개발자는 무료란다. 그럼 써야지!
<!--more-->

<br/>

## 0. 핵심요약

{{< admonition >}}
- 사용하는 툴에 AWS Toolkit 플러그인 설치하고, 로그인 하면 끝
- 개인 개발자는 무료
- 지원 언어 : Python, Java, JavaScript, TypeScript, C#. Rust, Go, Ruby, Scala, Kotlin, PHP, C, C++, Shell Scripting, SQL.
{{< /admonition >}}

<br/>

---

## 1. 유튜브 영상으로 느낌 알아보기
아래 영상 정도로 느낌을 알아보자.

[{유튜브 - AWS} Amazon CodeWhisperer Overview](https://www.youtube.com/watch?v=j8BoVmHKFlI)

{{< youtube j8BoVmHKFlI >}}

<br/>

[{유튜브 - 코드깍는노인} 코파일럿에 이어 CodeWhisperer까지! 코딩 너무 쉬워진다!](https://www.youtube.com/watch?v=lDeBfV16Lfo)

{{< youtube lDeBfV16Lfo >}}

<br/>

---

## 2. AWS Toolkit 플러그인 설치
참고 : https://docs.aws.amazon.com/codewhisperer/latest/userguide/whisper-setup-indv-devs.html

개인적으로 JetBrains의 IDE를 선호한다.  
PyCharm에 AWS Toolkit 플러그인을 설치해보자.  
(위 링크에 VS Code, JetBrains 둘 다 가이드가 잘 되어 있음)

<br/>

PyCharm 기준으로, 아래 메뉴에서 설치하면 된다.
- File > Settings
- Plugins > "Marketplace" 탭
- "Copilot" 검색
- "GitHub Copilot" 설치

{{< image src="/images/dev_common/code_whisperer_1.png" caption="플러그인 설치" width="1071">}}

<br/>

---

## 3. 계정 연결 및 CodeWhisperer 활성화
IDE 재시작하고 나면, AWS Toolkit 로그인이 필요하다.

{{< image src="/images/dev_common/code_whisperer_2.png" caption="AWS Builder ID 만들기" width="1023">}}

혹, 1번의 메뉴가 안보인다면, View > Tool Windows > AWS Toolkit 에 있다.  

AWS Builder ID 라는 걸 새로 만들어서 사용하게 된다.

<br/>

---

## 4. CodeWhisperer 도움 받기
공식 문서의 [Language support in Amazon CodeWhisperer](https://docs.aws.amazon.com/codewhisperer/latest/userguide/language-ide-support.html)에 따르면, Java, Python, JavaScript, TypeScript, C# 에서 활용하는 걸 추천하고 있다.

> In terms of the quality of the training data, the programming languages with the most support are:
> - Java
> - Python
> - JavaScript
> - TypeScript
> - C#

<br/>

기본적으로, 주석을 적당히 쓰면 추천을 해주고, `Tab` 을 누르면 해당 코드를 사용한다.

<br/>

Python 예시는 아니지만, 공식 페이지에서는 아래 느낌의 Java 예시를 보여주고 있다.

<br/>

가만히 있어도 뭔가 추천해 준다.

{{< image src="https://docs.aws.amazon.com/images/codewhisperer/latest/userguide/images/cw-jb-singeline-062022.png" caption="이미지 출처: [Single-line code completion](https://docs.aws.amazon.com/codewhisperer/latest/userguide/single-line-completion.html)" width="430">}}

<br/>

주석을 영어로 풀어서 써두면, 메소드 시그니쳐부터 추천을 해 준다.  

{{< image src="https://docs.aws.amazon.com/images/codewhisperer/latest/userguide/images/cw-jb-comment-062022.png" caption="이미지 출처: [Full function generation](https://docs.aws.amazon.com/codewhisperer/latest/userguide/whisper-full-function-generation.html)" width="402" >}}

<br/>

Docstring 을 잘 작성해 두면, 고거에 맞춰서 완성도 해 준다.

{{< image src="https://docs.aws.amazon.com/images/codewhisperer/latest/userguide/images/cw-jb-docstring-062022.png" caption="이미지 출처: [Docstring and Javadoc completion](https://docs.aws.amazon.com/codewhisperer/latest/userguide/whisper-docstring-javadoc.html)" width="659" >}}

<br/>

---

## 5. ETC

추가로, 요 블로그 [A Beginner's Guide to Prompt Engineering with GitHub Copilot](https://dev.to/github/a-beginners-guide-to-prompt-engineering-with-github-copilot-3ibp) 에 GitHub Copilot을 잘 활용하기 위한 팁이 몇 가지 있다. CodeWhisperer 에도 동일하게 적용할 수 있어 보인다. 주요 소제목은 아래와 같다.
- Provide high-level context followed by more detailed instructions
- Provide specific details
- Provide examples
- Iterate your prompts
- Keep a tab opened of relevant files in your IDE

<br/>

---

## 99. Reference
- [{공식페이지} Amazon CodeWhisperer](https://aws.amazon.com/codewhisperer/)
- [{공식페이지} Amazon CodeWhisperer Pricing](https://aws.amazon.com/codewhisperer/pricing/)
- [{AWS Docs} CodeWhisperer - Languages and IDEs](https://docs.aws.amazon.com/codewhisperer/latest/userguide/language-ide-support.html)
- [{AWS Docs} CodeWhisperer - Code examples](https://docs.aws.amazon.com/codewhisperer/latest/userguide/whisper-code-examples.html)
- [{유튜브 - AWS} Amazon CodeWhisperer Overview](https://www.youtube.com/watch?v=j8BoVmHKFlI)
- [{유튜브 - 코드깍는노인} 코파일럿에 이어 CodeWhisperer까지! 코딩 너무 쉬워진다!](https://www.youtube.com/watch?v=lDeBfV16Lfo)
- [{외국 블로그} A Beginner's Guide to Prompt Engineering with GitHub Copilot](https://dev.to/github/a-beginners-guide-to-prompt-engineering-with-github-copilot-3ibp)

<br/>

---