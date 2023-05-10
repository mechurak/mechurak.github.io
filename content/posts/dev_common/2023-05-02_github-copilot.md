---
title: "GitHub Copilot 맛보기 (feat. PyCharm)"
date: 2023-05-02T23:00:00+09:00
categories: [ 개발 일반 ]
tags:
- copilot
- jetbrains
---

AI가 코드 작성을 도와준단다. 한 달 무료라니까 일단 써 보자.
<!--more-->

## 1. 핵심요약

{{< admonition >}}
1. GitHub 로그인 후 Settings에서 Copilot enable 하기
2. 사용하는 툴에 GitHub Copilot 플러그인 설치
3. 툴에서 GitHub Login 
4. 주석이나 함수명 입력후 추천 나오면 `Tab`으로 적용
{{< /admonition >}}

<br/>

---



## 2. GitHub 에서 Copiot 활성화하기
참고: https://docs.github.com/en/copilot/quickstart

2023년 5월 2일 현재 1달 무료 플랜이 있다. 일단, 한 달 체험해보고 나중에 취소하든가 하자.

<br/>

---

## 3. PyCharm이나 AndroidStudio에 GitHub Copilot 플러그인 설치
참고: https://docs.github.com/en/copilot/getting-started-with-github-copilot

개인적으로 JetBrains 의 IDE를 선호한다.  
일단 PyCharm에 GitHub Copilot 플러그인을 설치해보자.

<br/>

아래 메뉴에서 설치하면 된다.
- File > Settings
- Plugins > "Marketplace" 탭
- "Copilot" 검색
- "GitHub Copilot" 설치

{{< image src="/images/dev_common/copilot_plugin.png" caption="플러그인 설치" width="990">}}


<br/>

IDE 재시작 되고 나면, GitHub에 로그인이 필요하다.
- Tools > GitHub Copilot > Login to GitHub

{{< image src="/images/dev_common/copilot_plugin_login.png" caption="로그인" width="529" >}}


<br/>

---

## 4. Copilot 도움 받기
[Get Started](https://docs.github.com/en/copilot/getting-started-with-github-copilot#seeing-your-first-suggestion) 페이지에 따르면, Python, JavaScript, TypeScript, Ruby, Go, C#, C++ 얘네들에 활용하는 걸 추천한다.

> GitHub Copilot provides suggestions for numerous languages and a wide variety of frameworks, but works especially well for Python, JavaScript, TypeScript, Ruby, Go, C# and C++.

<br/>

[Frequently asked questions](https://github.com/features/copilot/) 에는 아래 내용이 있다.

> **How does a customer get the most out of GitHub Copilot?**
> GitHub Copilot works best when you divide your code into small functions, use meaningful names for functions parameters, and write good docstrings and comments as you go. It also seems to do best when it’s helping you navigate unfamiliar libraries or frameworks.

함수를 작게 만들고, 함수 이름 잘 짓고, 주석 잘 적어 달랜다. 그 정돈 해 줘야지.

<br/>

이제 첫 시도이다.  
주석에 적당히 설명하면, 다음 이어질 주석 혹은 주석을 만들어 주려고 시도한다.

{{< image src="/images/dev_common/copilot_fisrt_try.png" caption="첫 번째 시도" width="389" >}}

`# Find all dollar amounts in a string` 까지 쓰니까, 밑에 줄은 얘가 추천해 줬고,
고 밑에 `def find_` 까지 쓰니까 아래 코드를 추천해 준다.  
요렇게 추천이 뜬 상태에서 `Tab` 을 누르면 해당 코드를 사용한다.  
`import` 는 따로 해줘야 하지만, 요정도면 훌륭하다.

<br/>

테스트 해보니 결과가 안 맞는게 있어서, 구현을 지우고 다시 추천을 받았다.  
저 오른쪽의 `GitHub Copilot` 탭에서 refresh 를 누르면 추천 리스트가 뜬다.

{{< image src="/images/dev_common/copilot_tab.png" caption="GitHub Copilot 탭" width="1225">}}

<br/>

잘 활용하면 유용한 파트너로 써먹을 수 있을 듯 하다.  

요 블로그 [A Beginner's Guide to Prompt Engineering with GitHub Copilot](https://dev.to/github/a-beginners-guide-to-prompt-engineering-with-github-copilot-3ibp) 에 활용을 잘하기 위한 팁이 몇 가지 있다. 일독해보면 좋을 듯 하다.  주요 소제목은 아래와 같다.
- Provide high-level context followed by more detailed instructions
- Provide specific details
- Provide examples
- Iterate your prompts
- Keep a tab opened of relevant files in your IDE

<br/>

---

## 99. Reference
- [{GitHub Features} Copilot](https://github.com/features/copilot/)
- [{GitHub Docs} Quickstart for GitHub Copilot](https://docs.github.com/ko/copilot/quickstart)
- [{GitHub Docs} Getting started with GitHub Copilot](https://docs.github.com/en/copilot/getting-started-with-github-copilot)
- [{외국 블로그} A Beginner's Guide to Prompt Engineering with GitHub Copilot](https://dev.to/github/a-beginners-guide-to-prompt-engineering-with-github-copilot-3ibp)
- [{GitHub Docs} Canceling your Copilot for Individuals subscription](https://docs.github.com/en/billing/managing-billing-for-github-copilot/managing-your-github-copilot-subscription-for-your-personal-account#canceling-your-copilot-for-individuals-subscription)


<br/>

---