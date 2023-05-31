---
title: "GitHub의 PR(Pull Request) 내용을 로컬에 적용하기"
date: 2023-05-31T23:00:00+09:00
categories: [ 개발 일반 ]
tags:
- git
---

GitHub의 한 프로젝트를 로컬에 당겨왔는데 오류가 있어서 PR에 있는 수정이 추가로 필요한 상황이다. 요럴 때 PR 번호로 로컬에 당겨올 수 있다.
<!--more-->

<br/>

## 0. 핵심요약

{{< admonition >}}
- 아래 커맨드로 특정 PR(`ID`)를 로컬의 새로운 브랜치(`NEW_BRANCH_NAME`)으로 당겨올 수 있다.
- `git fetch origin pull/{ID}/head:{NEW_BRANCH_NAME}`
{{< /admonition >}}

<br/>

## 1. 예시 상황

아래의 Unity 샘플 프로젝트를 살펴보고 싶었다.

{{< image src="https://camo.githubusercontent.com/a8f1cafeb432a4f96803c40434ce08150af45215163103499d0357fae2f8c4a4/68747470733a2f2f692e696d6775722e636f6d2f6d346375756c332e706e67" caption="[{GitHub - Unity} Unity Input System - 'Warriors' Example Project](https://github.com/UnityTechnologies/InputSystem_Warriors)">}}

<br/>

일단 아래 커맨드로 소스를 로컬에 받는다.
```bash
git clone --depth=1 https://github.com/UnityTechnologies/InputSystem_Warriors.git
```
- 히스토리는 관심 없으니 `--depth=1` 옵션으로 [shallow clone](https://mechurak.github.io/2023-05-29_git-shallow-clone/)

<br/>

실행해보니 에러가 살짝 발생하는데, Issues를 살펴보니 해당 상황을 해결하는 PR이 하나 올라와 있었다.
- [(#13) fix the playerinput.devices[0] references issue](https://github.com/UnityTechnologies/InputSystem_Warriors/pull/13)

<br/>

이럴때, 아래 커맨드로 해당 13번 PR을 로컬 브랜치에 받을 수 있다.
```bash
git fetch origin pull/13/head:awesome_fix
```
- 13번 PR 이므로 중간에 `{ID}` 자리에 `13`
- `{NEW_BRANCH_NAME}` 는 로컬에 만들 브랜치 이름이니까 적당히 `awesome_fix`

<br/>

이제 고 브랜치에 checkout 해서 계속 둘러보면 됨
```bash
git checkout awesome_fix
```

<br/>

---

## 99. Reference
- [{Stackoverflow} How can I check out a GitHub pull request with git?](https://stackoverflow.com/a/30584951)
- [{GitHub Docs} Checking out pull requests locally](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/)


<br/>

---