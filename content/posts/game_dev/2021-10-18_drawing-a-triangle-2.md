---
title: "'Command Buffers Overview - Vulkan Game Engine Tutorial 05 part 2' 정리"
date: 2021-10-18T23:30:00+09:00
summary: "command buffer 에 대해 알아보고, 드디어 삼각형 하나를 그립니다!"
categories: [ 게임 개발 ]
series: [ Vulkan Game Engine Tutorial 정리 ]
series_weight: 7
# featuredImage: /images/lve/i5-2_cover.jpg
tags:
- vulkan
---

LittleVulkanEngine 을 따라 만들어 보면서, Vulkan을 배워봅시다.


![cover](/images/lve/i5-2_cover.jpg)
[Youtube Link](https://youtu.be/_VOR6q3edig?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR)

<br/>

---


## 1. 주요내용

- Command Buffer 설명
- 삼각형 하나 드디어 그렸음
- 쉐이더 컴파일 자동화
- 고정 댓글에 PipelineConfigInfo 관련 [추가 수정](https://github.com/blurrypiano/littleVulkanEngine/commit/a867ab39c43ccc89ca744db84137df179b41daa7) 있음

<br/>

---

## 2. Command Buffer

Vulkan 에서는 gpu 가 수행할 명령들을 Command Buffer 라는 데 넣어서 gpu에게 submit 합니다.

### 2.1. 일반

Vulkan을 cmd를 바로 실행할 수 없습니다.

Command Buffer에 레코딩 해 놓고, Device Grphics Queue에 서밋하는 것으로 실행을 합니다.

[22s](https://youtu.be/_VOR6q3edig?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=22)
![i5_command_buffer.jpg](/images/lve/i5_command_buffer.jpg)

한 번 레코딩 해두고, 매 프레임마다 녹화 해둔 걸 다시 사용하는 거죠.

OpenGL 에서는 매 프레임 draw command를 부른다고 합니다.

### 2.2. Command Buffer Lifecycle

[58s](https://youtu.be/_VOR6q3edig?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=58)
![i5_command_buffer_lifecycle.jpg](/images/lve/i5_command_buffer_lifecycle.jpg)

커맨드 버퍼는 위와 같은 Lifecycle을 가집니다.

처음에 커맨드들을 녹화해두면 Executable 상태가 되고, Queue에 서밋하면 Pending, 사용이 완료되면 다시 Executable 상태가 됩니다.

[97s](https://youtu.be/_VOR6q3edig?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=97)
![i5_command_buffer_fence.jpg](/images/lve/i5_command_buffer_fence.jpg)

Framebuffer가 3개여도 Command Buffer는 두 개면 충분하기는 하다고 합니다.

저희는 배우는 단계니까 1:1 로 갑니다.

### 2.3. VkCommandBufferAllocateInfo

[VkCommandBufferAllocateInfo](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/VkCommandBufferAllocateInfo.html) 를 채워서 VkCommandBuffer를 생성하는 데 사용합니다.


[174s](https://youtu.be/_VOR6q3edig?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=174)
![i5_command_buffer_level.jpg](/images/lve/i5_command_buffer_level.jpg)

이 중에 level 설정은 Command Buffer의 성격을 결정 합니다.
- Primary : Device Graphics Queue 에 서밋 될 수 있음
- Secondary : 서밋은 안됨. 다른 Primary 커맨드 버퍼에서 실행할 수 있음

<br/>

[503s](https://youtu.be/_VOR6q3edig?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=503)
![i5_command_buffer_mix.jpg](/images/lve/i5_command_buffer_mix.jpg)

Command Buffer 안에는 위의 그림과 같이 커맨드들이 레코딩됩니다.

```
Start Render Pass  # 어떤 구조 포맷의 프레임버퍼인지 설정
  Bind Pipeline 1  # 파이프라인에 바인딩
  Draw 100 vertices  # 실제 그럼
  ...
End Render Pass  # 해당 렌더 패스 끝
```

위와 같이 Render Pass 안에 실제 커맨드가 있는 경우 Inline 이라고 합니다.  
Render Pass 안에 실제 커맨드 대신 vkCmdExecute() 로 Secondary Command Buffer를 실행할 수도 있습니다.

단, 요 두가지는 섞어서 쓸 수는 없다고 합니다.

---

## 3. 구현

### 3.1. FirstApp 구현

- createCommandBuffers() 구현  
  - VkCommandBuffer 두 개 생성  
  - 고 버퍼들에 명령 녹화
    - 녹화 시작 (vkBeginCommandBuffer())  
    - 렌더 패스 준비 (VkRenderPassBeginInfo)  
    - 렌더 패스 시작 (vkCmdBeginRenderPass())  
    - 그래픽 파이프라인 바인드  
    - vkCmdDraw() 호출  
    - 렌더 패스 끝 (vkCmdEndRenderPass())  
    - 녹화 끝 (vkEndCommandBuffer())  
- drawFrame() 구현  
  - 이미지 인덱스 가져옴 (from swap chain)  
  - 커맨드 버퍼 서밋
  
- run() 에 기다리는 코드 추가

### 3.2. LivePipeline 구현

- bind(VkCommandBuffer) 메소드 추가
  - vkCmdBindPipeline() : 커맨드 버퍼를 VkPipeline 에 바인드함

---

## 4. 쉐이더 컴파일 자동화

비주얼 스튜디오를 위한 설명을 없네요.

나중에 찾아보도록 하겠습니다.

---

## 5. 다이어그램

![i5_diagram.png](/images/lve/i5_diagram.png)

여기까지 전체 동작을 한 번 훑어 봅시다.

FirstApp 인스턴스가 만들어지면,

1. LveWindow 객체를 만듭니다.

2. LveDevice 객체를 만듭니다.

3. LveSwapChain 객체를 만듭니다.

4. FirstApp 생성자를 호출합니다.
    - pipelineLayout 을 만듭니다.
    - LvePipeLine 객체를 만듭니다.
    - Command Buffers 를 만들고 레코딩 합니다.

<br/>

이후에, run() 루프에서 매프레임 command buffer를 서밋하여, gpu에게 삼각형를 그리라고 시킵니다.

<br/>

---