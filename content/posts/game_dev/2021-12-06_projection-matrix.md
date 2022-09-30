---
title: "'Projection Matrices - Vulkan Game Engine Tutorial 13' 정리"
date: 2021-12-06T23:30:00+09:00
summary: "Projection matrix를 통해 어떤 유형의 카메라를 이용할지 결정합니다."
categories: [ 게임 개발 ]
series: [ Vulkan Game Engine Tutorial 정리 ]
# featuredImage: /images/lve/i13_cover.jpg
tags:
- vulkan
---

LittleVulkanEngine 을 따라 만들어 보면서, Vulkan을 배워봅시다.


![cover](/images/lve/i13_cover.jpg)
[Youtube Link](https://youtu.be/YO46x8fALzE?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR)

<br/>

---


## 1. 주요내용

- LveCamera 클래스 추가 (Perspective matrix, Orthographic matrix 처리)
- 창 사이즈를 바꿔도 aspect ratio를 유지하도록 함

<br/>

---

## 2. 이론 설명

### 2.1. 직교투영과 원근투영

![i12-1_overall.jpg](/images/lve/i12-1_overall.jpg)
월드 좌표계에 있는 물체를 카메라에 입장에서 Vulkan's Canonical Viewing Volume `x(-1, 1), y(-1, 1), z(0, 1)` 이라는 데로 가져와야 합니다.
원근감 없이 그냥 가져오면 orthographic projection 이라고 합니다.
원근감을 고려해서 절두체에서 orthographic view volume을 거쳐서 오면 perspective projection 이 됩니다.

<br/>

몇 개의 변수만 정하면 저 매트릭스는 정해지는 것 같으니, 자세한건 별도로 준비된 영상을 참고합시다.

![i12-1_cover.jpg](/images/lve/i12-1_cover.jpg)
[The Math behind (most) 3D games - Perspective Projection](https://www.youtube.com/watch?v=U0_ONQQ5ZNM&list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR)


<br/>

orthographic projection 과 perspective projection 의 결과물은 요렇게 보입니다.
![i13_difference.jpg](/images/lve/i13_difference.jpg)

<br/>

### 2.2. aspect ratio 유지

지금은 창을 늘리면 큐브도 늘어나는데, 가로 세로 제각각 늘어납니다.
Canonical View Volume 이 윈도우 사이즈에 딱 맞도록 세팅되어 있기 때문입니다.
![i13_viewing_volume.jpg](/images/lve/i13_viewing_volume.jpg)


요걸 aspect ratio 를 유지하도록 하려면, Orthographic View Volume 을 적절히 조정해야 합니다.
[407s](https://youtu.be/YO46x8fALzE?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=407)
![i13_aspect.jpg](/images/lve/i13_aspect.jpg)


<br/>

---

## 3. 코드 수정

### 3.1. LveCamera 클래스 추가
**setOrthographicProjection()**
[65s](https://youtu.be/YO46x8fALzE?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=65)
![i13_ortho.jpg](/images/lve/i13_ortho.jpg)
- 직교투영을 사용하도록 세팅
- 윈도우 크기가 변할 수 있으므로(aspect 가 바뀔 수 있으므로), FirstApp:run()의 루프에서 매 프레임 세팅

<br/>

**setPerspectiveProjection()**
[85s](https://youtu.be/YO46x8fALzE?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=85)
![i13_perspective.jpg](/images/lve/i13_perspective.jpg)
- 원근투영을 사용하도록 세팅
- 윈도우 크기가 변할 수 있으므로(aspect 가 바뀔 수 있으므로), FirstApp:run()의 루프에서 매 프레임 세팅

<br/>

**getProjection()**
- Projection Matrix 리턴
- SimpleRenderSystem::renderGameObjects() 에서 게임 오브젝트를 위한 push constant 를 세팅할 때 사용

<br/>

### 3.2. LveRenderer 클래스 수정
- getAspectRatio() 추가 : swapchain의 aspect ratio 를 가져다가 쓰기 위함

<br/>

### 3.3. SimpleRenderSystem 클래스 수정
- renderGameObjects() 메소드에서 LveCamera& 도 같이 받음
- `push.tansform` push constant 만들 때, projection matrix 도 곱해 줌
- 일단은 cpu 에서 곰셉 (보통은 gpu 에서 하지만)
- 현재는 카메라가 원점에 위치하고 있음

<br/>


### 3.4. 클래스 다이어그램
![i13_diagram.png](/images/lve/i13_diagram.png)


<br/>

---