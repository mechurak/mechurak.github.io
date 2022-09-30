---
title: "'Renderer & Systems - Vulkan Game Engine Tutorial 11' 정리"
date: 2021-11-25T23:30:00+09:00
summary: "Renderer와 SimpleRenderSystem을 별도의 클래스로 뽑아냅니다."
categories: [ 게임 개발 ]
series: [ Vulkan Game Engine Tutorial 정리 ]
series_weight: 11
# featuredImage: /images/lve/i11_cover.jpg
tags:
- vulkan
---

LittleVulkanEngine 을 따라 만들어 보면서, Vulkan을 배워봅시다.


![cover](/images/lve/i11_cover.jpg)
[Youtube Link](https://youtu.be/uGRSTRGlZVs?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR)

<br/>

---


## 1. 주요내용

- 클래스 구조 리팩토링
- LveRenderer 클래스 추가
  - FirstApp 클래스에서 SwapChain, Command Buffers, draw frame 관련 부분 뽑아냄
- SimpleRenderSystem 클래스 추가
  - FirstApp 클래스에서 Pipeline, Pipeline Layout, render GameObjects 관련 부분 뽑아냄

<br/>

---

## 2. 코드 수정

### 2.1. 개요

지금까지는 FirstApp에 너무 많은 기능이 몰려 있습니다.
요번 강에서 렌더링 관련 부분을 담당하는 클래스를 좀 떼어냅니다.


[53s](https://youtu.be/uGRSTRGlZVs?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=53)
![i11_new_class.jpg](/images/lve/i11_new_class.jpg)

FisrtApp 의 기능 중 일부를 떼어내서
SwapChain, Command Buffers, draw Frame 을 담당하는 `LveRenderer` 클래스와,
Pipeline, Pipeline Layout, Push Constant 구조체, render GameObjects 를 담당하는 `SimpleRenderSystem`을 만들 겁니다.

<br/>

[77s](https://youtu.be/uGRSTRGlZVs?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=77)
![i11_system.jpg](/images/lve/i11_system.jpg)

Renderer 는 하나만 존재하고, System 은 여러 종류가 있을 수 있습니다.
(여기서의 "System" 은 ECS에서의 System을 의미하지는 않습니다.)


<br/>

[131s](https://youtu.be/uGRSTRGlZVs?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=131)
![i11_system2.jpg](/images/lve/i11_system2.jpg)
(아마도 GameObject는 그냥 각 System을 다 호출하고, System에서 분기를 하는 게 정석인 듯 하지만)
우리는 단순함을 위해, 호출부에서 특정 System을 호출해도 문제 없는지 잘 판단해서 호출하는 걸로 합니다.

<br/>

### 2.2. LveRenderer 클래스 추가
- FirstApp에서 SwapChain, Command Buffers, draw Frame 관련 로직 옮겨 옵니다.

<br/>

### 2.3. SimpleRenderSystem 클래스 추가
- FirstApp에서 Pipeline, Pipeline Layout, Push Constant, render GameObjects 관련 로직 옮겨 옵니다.

<br/>

### 2.4. 결과 화면
![i10_result.png](/images/lve/i10_result.png)
10강과 같은 동작으로 삼각형이 오른쪽(시계 방향)으로 회전합니다.


---


## 3. 다이어그램

### 3.1. 클래스 다이어그램
![i11_diagram.png](/images/lve/i11_diagram.png)

아래 시퀀스 다이어그램과 같이 보면 좋습니다.

<br/>

### 3.2. 시퀀스 다이어그램

![i11_sq_init.png](/images/lve/i11_sq_init.png)

<br/>

![i11_sq_loop.png](/images/lve/i11_sq_loop.png)


<br/>

---