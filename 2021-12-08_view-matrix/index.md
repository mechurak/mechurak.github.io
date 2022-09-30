# 'Camera (View) Transform - Vulkan Game Engine Tutorial 14' 정리


LittleVulkanEngine 을 따라 만들어 보면서, Vulkan을 배워봅시다.


![cover](/images/lve/i14_cover.jpg)
[Youtube Link](https://youtu.be/rvJHkYnAR3w?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR)

<br/>

---


## 1. 주요내용

- LveCamera 클래스에 view matrix 추가
- `push.tansform` push constant 만들 때, projection * view * model 매트릭스 다 곱한 값 사용

<br/>

---

## 2. 이론 설명

### 2.1. overall

![i14_camera_tr.jpg](/images/lve/i14_camera_tr.jpg)

우리가 그려야할 3차원 오브젝트는 각자의 좌표계를 가지고 있습니다. 보통 중앙이나 발 밑이 원점이 되는 것 같습니다. 실제로 pipeline 에는 요 좌표계 기준의 vertex 데이터들이 들어갑니다.

<br/>

- Object Space - `Model Transform` -> World Space

각 오브젝트를 월드 좌표계에 배치하는 작업입니다. 스케일, 회전도 같이 세팅 합니다.
각 객체마다 월드 좌표계에서 자신의 위치가 있으므로, GameObject 각자마다 자신의 (월드 좌표계에서) 위치를 가지고 있으면 됩니다.

<br/>

- World Space - `Camera(View) Transform` -> Camera Space

장면을 바라볼 카메라도 움직일 수 있습니다. 최종 화면에 보일 결과물도 카메라 기준으로 그려져야 합니다. 고로, 카메라가 원점에 위치 되게 좌표계를 조정합니다.

<br/>

- Camera Space - `Projection Transform` -> Canonical View Volume

어느 영역을 보이는 영역으로 할 건지, 직교투영을 사용할 건지, 원근 투영을 사용할 건지를 정해서, 해당 영역을 Canonical View Volume 영역으로 매핑합니다.

<br/>

- Canonical View Volume - `Viewport Transform` -> Viewport

Canonical View Volume 에 들어와 있는 객체들을 Viewport 에 매핑합니다. 요건 pipeline 에서 자동으로 하는 것 같습니다.

<br/>

### 2.2. 사과 GameObject

사과 GameObject 의 예를 봅시다.

![i14_overall.jpg](/images/lve/i14_overall.jpg)

model transform 을 통해 월드 좌표계에 배치하고, projection transform 을 통해 canonical view volume 에 매핑합니다.
이후 viewport transform 을 통해 화면에 그려집니다.

<br/>

---

## 3. 코드 수정

### 3.1. LveCamera 클래스 수정
- viewMatrix 변수 추가 (mat4) : view matrix 갖고 있음
- viewMatrix 를 세팅할 유틸 함수들 추가

<br/>

### 3.2. FirstApp::run() 메소드 수정
- while loop 진입하기 전에 camera 의 view matrix 세팅

<br/>

### 3.3. SimpleRenderSystem::renderObjects() 메소드 수정
- for loop 진입하기 전에 projection matrix 와 view matrix 곱해 둠
- `push.tansform` push constant 만들 때, projection * view * model 매트릭스 다 곱한 값 사용

<br/>

### 3.4. 클래스 다이어그램
![i14_diagram.jpg](/images/lve/i14_diagram.jpg)

<br/>

### 3.5. 시퀀스 다이어그램

![i14_sq_init.png](/images/lve/i14_sq_init.png)

<br/>

![i14_sq_loop.png](/images/lve/i14_sq_loop.png)

<br/>

---
