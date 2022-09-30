---
title: "안드로이드에서의 MVC, MVP, MVVM"
date: 2021-08-22T01:01:00+09:00
summary: "MVVM 에 맞춰서 앱 구조를 잡도록 하자"
categories: [ android ]
# featuredImage: /images/logo/android.png
tags:
- android
- architecture
---

## 한 줄 요약

MVVM 에 맞춰서 앱 구조를 잡도록 하자.

<br/>

---

## 들어가며

안드로이드에서 [권장되는 앱 아키텍처](https://developer.android.com/jetpack/guide#overview)는 아래의 모습입니다.

![권장 앱 아키텍처](https://developer.android.com/topic/libraries/architecture/images/final-architecture.png)

MVVM (Model - View - ViewModel) 패턴을 기본으로 하고 있습니다.

요 MVVM 패턴을 권장하기 전에는 특별한 가이드가 없었고, MVC, MVP 패턴이 많이 쓰였다고 합니다.

안드로이드에서 MVC, MVP, MVVM 패턴을 각각 적요하면 어떻게 구현이 되는지 정리해 준 블로그가 있어서, 다이어그램을 그리면서 따라가 보았습니다.

![android-app-architecture](/images/android/android-app-architecture.png)

<br/>

---

## MVC

- `Activity`가 `Controller` 역할
- `Controller` 쪽 TC 만들기가 애매함
- `Controller`가 비대해지기 쉬움
- 간단한 프로그램에 적합

<br/>

---

## MVP

- `Activity`가 `View` 역할
- `Presenter` 에는 안드로이드 API 사용이 없는 것이 좋음
- `Presenter` 테스트가 용이해 짐
  - `TicTackToeView` 만 mocking 혹은 fake 객체 만들면 됨
- `Presenter`가 비대해지기 쉬움

<br/>

---

## MVVM

- 의존 관계가 한 방향으로만 있음
- 모듈화와 테스트가 용이함
- [Data Binding](https://developer.android.com/topic/libraries/data-binding/index.html)을 사용하면 `View` 코드가 간결해짐
- [ViewModel](https://developer.android.com/topic/libraries/architecture/viewmodel), [LiveData](https://developer.android.com/topic/libraries/architecture/livedata) 사용함으로써 Lifecycle 관련 처리도 같이

<br/>

---

## Reference

- [Guide to ap architecture / 안드로이드 공식 문서](https://developer.android.com/jetpack/guide#overview)
- [MVC vs. MVP vs. MVVM on Android\_Eric Maxwell\_2017-01-26](https://academy.realm.io/posts/eric-maxwell-mvc-mvp-and-mvvm-on-android/)
- [위 글의 한글 버전](https://academy.realm.io/kr/posts/eric-maxwell-mvc-mvp-and-mvvm-on-android/)
- [위 글에서 사용한 소스 코드 / Github](https://github.com/ericmaxwell2003/ticTacToe)

<br/>

---
