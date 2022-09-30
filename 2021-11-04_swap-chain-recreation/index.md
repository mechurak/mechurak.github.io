# 'Swap Chain Recreation & Dynamic Viewports - Vulkan Game Engine Tutorial 08' 정리


LittleVulkanEngine 을 따라 만들어 보면서, Vulkan을 배워봅시다.


![cover](/images/lve/i8_cover.jpg)
[Youtube Link](https://youtu.be/0IIqvi3Z0ng?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR)

<br/>

---


## 1. 주요내용

- 윈도우 사이즈 바뀌었을 때 처리 추가
  - SwapChain 재생성
  - pipeline에서 dynamic viewport 사용

<br/>

---

## 2. 윈도우 사이즈 변경에 따른 변경
윈도우 사이즈가 바뀌면(framebuffer 의 크기가 달라지면), SwapChain을 다시 만들어야 합니다.

[33s](https://youtu.be/0IIqvi3Z0ng?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=33)
![i8_invalid.jpg](/images/lve/i8_invalid.jpg)

현재로써는 swapchain의 width, height 에 디펜던시가 있는 pipeline 도 다시 만들어야 합니다.

<br/>

원래는 swap chain 이 바뀌어도 RenderPass 가 compatible 하다면(타겟 프레임 버퍼의 형식이 바뀐게 아니라면), 기존에 만들었던 pipeline을 그대로 쓸수 있다고 합니다.  
(이번 강의의 경우처럼 단순한 윈도우 리사이즈 같은 경우를 말하는 것 같습니다.)
[773s](https://youtu.be/0IIqvi3Z0ng?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=773)
![i8_renderpass_compatible.jpg](/images/lve/i8_renderpass_compatible.jpg)

나중에 그렇게 하기 위해, pipeline 에서 swapchain 으로의 디펜던시(화면 크기)를 없앱니다.
(dynamic viewport 사용)

pipeline을 재사용 하는 건 나중에 할 겁니다 ㅎ
지금은 그냥 다시 만듭시다.


std::move() 도 한 번 알아보도록 합시다. ([learncpp.com](https://www.learncpp.com/cpp-tutorial/stdmove/))

<br/>

---


## 3. 수정 내용 요약
[1056s](https://youtu.be/0IIqvi3Z0ng?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=1056)
![i8_summary.jpg](/images/lve/i8_summary.jpg)

- glfw 단에 윈도우의 사이즈 바뀌는 것에 콜백 등록
- 매 프레임마다, 윈도우 사이즈가 바뀌었는지, swapChain이 valid 한지 확인
- 파이프라인에서 dynamic viewport 사용 → swapChain의 크기에 대한 dependency 없어짐
- TODO: Render pass 의 compatibility 체크해서 pipeline 재사용 (나중에 할 거임)


<br/>

---


## 4. 구현
### 4.1. LveWindow 수정
- width, height 에서 const 제거
- framebufferResized 플래그 추가
- 윈도우 리사이즈 콜백을 받기 위한 framebufferResizedCallback() 클래스 메소드 추가


### 4.2. FirstApp 수정
- recreateSwapChain() 추가
  - 윈도우 리사이즈 콜백 받았을 때 불러서, lveSwapChain 새로 생성 및 createPipeline() 호출
- recordCommandBuffer() 추가
  - 매 프레임 커맨트 버퍼를 레코딩하기로 함
  - lvePipeline 에 bind()하기 전에, viewport와 scissor 세팅
- lveSwapChain 멤버 변수에 스마트 포인터 사용
- drawFrame() 수정
  - swapchain에 acquireNextImage() 가 실패하거나, submitCommandBuffers() 가 실패할 경우, recreateSwapChain()
- freeCommandBuffers() 추가


### 4.3. LvePipeline 수정
- dynamicStateInfo 채움 (VkPipelineDynamicStateCreaeInfo)
  - viewport 와 scissor 를 다이나믹하게 사용
- defaultPipelineConfigInfo() 에서 width, height 사용하지 않음



### 4.4. LveSwapChain 수정
- previous Swapchain 도 받는 생성자 추가
  - createSwapChain() 에서 기존 SwapChain 넘김
- init() 추가
- oldSwapChain 멤버 추가

<br/>

---


## 5. 다이어그램

### 5.1. 클래스
![i8_diagram.png](/images/lve/i8_diagram.png)

1. LveWindow 생성
2. LveDevice 생성
3. FirstApp() 생성자 호출
    4. LveModel 생성
    5. LveSwapChain 생성
    6. LvePipeline 생성

<br/>

이후, FirstApp::run() 에서 매프레임마다 commandBuffer 레코딩 및 서밋.


### 5.2. 시퀀스
FirstApp 클래스의 인스턴스 생성 부분입니다.

![i8_sq_init1.png](/images/lve/i8_sq_init1.png)
![i8_sq_init2.png](/images/lve/i8_sq_init2.png)


<br/>

다음은 run()의 루프 부분입니다.


![i8_sq_loop1.png](/images/lve/i8_sq_loop1.png)
![i8_sq_loop2.png](/images/lve/i8_sq_loop2.png)
![i8_sq_loop3.png](/images/lve/i8_sq_loop3.png)


<br/>

---
