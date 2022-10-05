---
title: "'Vertex Buffers - Vulkan Game Engine Tutorial 06' 정리"
date: 2021-10-26T23:30:00+09:00
summary: "vertex buffer에 대해 알아보고, LveModel 클래스를 만듭니다."
categories: [ 게임 개발 ]
series: [ Vulkan Game Engine Tutorial 정리 ]
series_weight: 8
# featuredImage: /images/lve/i6_cover.jpg
tags:
- vulkan
---

LittleVulkanEngine 을 따라 만들어 보면서, Vulkan을 배워봅시다.


![cover](/images/lve/i6_cover.jpg)
[Youtube Link](https://youtu.be/mnKp501RXDc?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR)

<br/>

---


## 1. 주요내용

- Vertext Buffer 설명
- LveModel 클래스 추가  

<br/>

지금까지는 vertex data 를 vertex shader 안에다 하드코딩 했습니다.  
요걸 쉐이더 코드 밖으로 빼내도록 합시다.

<br/>

---

## 2. Vertex Buffer 설명

Vertex buffer는 커다란 데이터 덩어리입니다.  
하나의 vertex 에는 여러 attribute가 있을 수 있는데, 여러 vertex의 데이터를 어떻게 묶을 것인가를 생각하보면,

vertex 별로 모아 놓거나, attribute 별로 모아 둘 수 있습니다.

### 2.1. per vertex (or per instance)

[97s](https://youtu.be/mnKp501RXDc?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=97)
![i6_vertex_buffer_per_vertex.jpg](/images/lve/i6_vertex_buffer_per_vertex.jpg)


여러 attribute 가 있을 때, vertex 별로 묶어서 두는 방법입니다.  
position(x, y), color(r,g,b) 가 vertex의 attribute 로 있는 경우의 예 입니다.

### 2.2. separate binding

[115s](https://youtu.be/mnKp501RXDc?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=115)
![i6_separete_binding.jpg](/images/lve/i6_separete_binding.jpg)


한 종류의 데이터만 가지는 Vertex Buffer 여러 개 만들어서 각각 바인딩을 하는 방법도 있습니다.  
각 attribute 별로 모여 있게 두는 방법입니다.

### 2.3. Vertex Binding Descriptions

어떤 조합도 가능하긴 합니다.  
각 Vertex Buffer 마다 바인딩 정보를 알려줘야 합니다.

![i6_binding_description.jpg](/images/lve/i6_binding_description.jpg)

binding 0번, stride(한 vertex에 해당하는 토막이 몇 byte인지) 20

### 2.4. Vertex Attribute Descriptions

각 attribute 에 대한  description도 있어야 합니다.

[195s](https://youtu.be/mnKp501RXDc?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=195)
![i6_vertex_attribute_description.jpg](/images/lve/i6_vertex_attribute_description.jpg)


binding, location, offset, format 을 알려줘야 합니다.

위 그림에서 `color` attribute의 경우를 보면,  
- bionding은 0 : 0번으로 binding 된 Vertex Buffer임
- location은 1 : Vertex Shader 로 넘길 때, 1번 위치임, 0번은 `position`
- offset은 8 : 앞의 position이 4bytes 정수 2개로 이루어져 있어서, 8bytes 지난 부분에 color 데이터 있음
- format은 R32G32B32 : 4byte float 3개 필요함

format은 컬러가 아니어도(position) 컬러 포맷을 사용합니다.  
아래 포맷들이 자주 쓰인다고 합니다.

![i6_common_format.jpg](/images/lve/i6_common_format.jpg)

### 2.5. Binding vs Buffer

[258s](https://youtu.be/mnKp501RXDc?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=258)
![i6_binding_vs_buffer.jpg](/images/lve/i6_binding_vs_buffer.jpg)


메모리 alloc 을 줄이기 위해, 메모리를 크게 잡고 바인딩을 여러 개 할 수도 있습니다.

### 2.6. Interleaved vs Separate binding?

[295s](https://youtu.be/mnKp501RXDc?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=295)  
![i6_vs.jpg](/images/lve/i6_vs.jpg)

간단한 상황에서 보통은 싱글 interleaved 가 추천된다고 합니다.  
물론, 상황이 복잡해지면 separate binding 이 나을 수도 있습니다.

### 2.7. 메모리풀

gpu 메모리 할당은 비싼 작업이라서, 메모리풀을 만들어서 쓰는 게 좋다고 합니다.  
나중에 언젠가 [Vulkan Memory Allocator](http://kylehalladay.com/blog/tutorial/2017/12/13/Custom-Allocators-Vulkan.html) 도 한 번 보도록 합시다.

우리는 그냥 `LveDevice::createBuffer()` 사용할 겁니다.

<br/>

---


## 3. 구현

### 3.1. LveModel 클래스 추가

- VkBuffer, VkDeviceMemory 따로 있음  
- create GPU 쪽에 메모리 만들어서, 거기다가 vertex data 복사

<br/>

[1052s](https://youtu.be/mnKp501RXDc?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=1052)
![i6_memory.jpg](/images/lve/i6_memory.jpg)

- bind() : [vkCmdBindVertexBuffers()](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkCmdBindVertexBuffers.html) 호출 (커맨드 버퍼에 해당 Vertex Buffer 바인딩)

- draw() : [vkCmdDraw()](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkCmdDraw.html) 호출

### 3.2. LveModel::Vertex 구조체 추가

- position(vec2) 정보 가짐
- static getBindingDescriptions() : vertex 바인딩 디스크립션 리턴  
- static getAttributeDescription() : vertex 어트리뷰트 디스크립션 리턴

### 3.3. simple_shader.vert

- 하드 코딩했던 vertex data 제거

### 3.4. LvePipeline에서 LveModel 사용

- vertexInputInfo 채움

### 3.5. FirstApp 수정

- loadModels() 추가: 여기다 vertex data 하드코딩. LveModel 인스턴스 만듬
- CommandBuffer 에서 vkCmdDraw() 제거하고, lveModel의 bind(), draw() 사용


<br/>

---


## 4. 다이어그램

![i6_diagram.png](/images/lve/i6_diagram.png)

1. LveWindow 생성
2. LveDevice 생성
3. LveSwapChain 생성
4. FirstApp() 생성자 호출
    5. LveModel 생성
    6. LvePipeline 생성

<br/>

이후, FirstApp::run() 에서 매프레임마다 swapChain에게 녹화해둔 commandBuffer 서밋.

<br/>

---
