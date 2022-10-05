---
title: "'Device Setup & Pipeline cont. - Vulkan Game Engine Tutorial 03' 정리"
date: 2021-10-15T23:30:00+09:00
summary: "device 관련 준비된 소스 다운 받아서 적용 (이해는 나~중에)"
categories: [ 게임 개발 ]
series: [ Vulkan Game Engine Tutorial 정리 ]
series_weight: 4
# featuredImage: /images/lve/i5-2_cover.jpg
tags:
- vulkan
---

[Youtube Link](https://www.youtube.com/watch?v=LYKlEIzGmW4&list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR)

{{< youtube LYKlEIzGmW4 >}}


## 1. 주요 내용

- device 관련 준비된 소스 다운 받아서 적용 (이해는 나~중에)
- [Vulkan-Tutorial.com](http://https://vulkan-tutorial.com/Drawing_a_triangle/Setup/Instance) 의 아래 섹션들에 해당

![triangle](/images/lve/i3_vulkan-tutorial.png)

## 2. 실습 코드 수정

### 2.1. LveWindow

- createWindowSurface() 준비 : LveDevice.createSurface() 에서 사용

### 2.2. LveDevice

- 생성자 동작
  1. Initializing vulkan and picking a physical device  
  2. Setup validation layers to help debug

### 2.2. LvePipeline

- PipelineConfigInfo : 앱단에서 파이프라인 주물럭 하기 위해 필요  
- 생성자 시그니처 변경  
- 멤버 변수들 추가  
- createShaderModule() 메소드 추가  
- static PipelineConfigInfo defaultPipelineConfigInfo() 추가

### 2.3. FirstApp

- LveDevice 멤버 추가  
- LvePipleline 시그니처 변경 적용

## 3. 다이어그램

![triangle](/images/lve/i3_class-diagram.png)
