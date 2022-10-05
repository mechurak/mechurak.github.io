---
title: "'Opening a window - Vulkan Game Engine Tutorial 01' 정리"
date: 2021-10-13T23:00:00+09:00
summary: "기존 셋업 코드에서 FirstApp, LveWindow 클래스 작성"
categories: [ 게임 개발 ]
series: [ Vulkan Game Engine Tutorial 정리 ]
series_weight: 2
# featuredImage: /images/lve/i5-2_cover.jpg
tags:
- vulkan
---

[Youtube Link](https://youtu.be/lr93-_cC8v4?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR)

{{< youtube lr93-_cC8v4 >}}

## 1. 주요 내용

- `LveWindow` 클래스 만듬: glfw 윈도우 캡슐화
- `FirstApp` 클래스 만듬: 앱의 entry 포인트

## 2. 클래스 다이어그램

![dartpad1.jpg](/images/lve/i1_class-diagram.png)

특별한 내용 없이, 기존의 셋업 코드에서 위 두 클래스를 만들었습니다.

앱이 실행되면, `FirstApp::run()` 이 실행됩니다.
