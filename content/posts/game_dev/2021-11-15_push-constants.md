---
title: "'Push Constants - Vulkan Game Engine Tutorial 09' 정리"
date: 2021-11-15T23:30:00+09:00
summary: "Push Costants를 이용해서 작은 상수 값을 쉐이더에 전달할 수 있습니다."
categories: [ 게임 개발 ]
series: [ Vulkan Game Engine Tutorial 정리 ]
series_weight: 9
# featuredImage: /images/lve/i9_cover.jpg
tags:
- vulkan
---

LittleVulkanEngine 을 따라 만들어 보면서, Vulkan을 배워봅시다.


![cover](/images/lve/i9_cover.jpg)
[Youtube Link](https://youtu.be/wlLGLWI9Fdc?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR)

<br/>

---


## 1. 주요내용

- Push Constants 는 쉐이더에 작은 상수 값을 전달하기 위한 방법
- 따로 메모리에 쓰거나 카피하지 않고, Vulkan Command Buffer를 사용함
- 자주 업데이트 되는 데이터에 유용하지만, 사이즈 제한 있음

<br/>

---

## 2. Push Constant 설명

### 2.1. 존재의 이유

커맨드 버퍼를 통해 쉐이더에 상수 값들을 전달할 수 있습니다.
[8s](https://youtu.be/wlLGLWI9Fdc?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=8)
![i9_pushConstants.jpg](/images/lve/i9_pushConstants.jpg)

<br/>

같은 모델에다 다른 상수를 적용해서 여러 번 그리는 게 가능합니다.
[70s](https://youtu.be/wlLGLWI9Fdc?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=70)
![i9_pushConstants2.jpg](/images/lve/i9_pushConstants2.jpg)


<br/>

transform 매트릭스 넘기는데 많이 사용한다고 합니다.
[757s](https://youtu.be/wlLGLWI9Fdc?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=757)
![i9_transform.jpg](/images/lve/i9_transform.jpg)

요거는 Linear Algebra 이해가 좀 필요한데, 아래 유튜브 영상을 추천했습니다.
- [3Blue1Browns Linear algebra series](https://www.youtube.com/playlist?list=PLZHQObOWTQDPD3MizzM2xVFitgF8hE_ab)

<br/>


### 2.2. 단점
사이즈 제한 있습니다. (128bytes 만 개런티 함)


<br/>


### 2.3. VkPushConstantRange 구조체
PipelineLayout 만들때 세팅해줘야 합니다.
(이런 push constant 를 사용할꺼야 라고 알려주는 느낌)


. stage flags : 어떤 쉐이더에서 접근할 것인가?
[119s](https://youtu.be/wlLGLWI9Fdc?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=119)
![i9_stageFlags.jpg](/images/lve/i9_stageFlags.jpg)


. offset : separate 방식으로 쓸 경우 필요
. size : 사이즈
[132s](https://youtu.be/wlLGLWI9Fdc?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=132)
![i9_offset_size.jpg](/images/lve/i9_offset_size.jpg)


<br/>


### 2.4. Separate vs Shared
쉐이더별로 push constant 를 구분하는 것보다, 여러 쉐이더에서 같이 쓰는걸 추천 한다고 합니다.
[159s](https://youtu.be/wlLGLWI9Fdc?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=159)
![i9_sharedRange.jpg](/images/lve/i9_sharedRange.jpg)



<br/>

---


## 3. 코드 수정
### 3.1. FirstApp 클래스 수정

**(1) lve 네임스페이스에 SimplePushConstantData 구조체 추가**

```cpp
struct SimplePushConstantData {  
   glm::vec2 offset;  
   alignas(16) glm::vec3 color;  
};
```

. 쉐이더 코드에서 해석을 위해, 메모리 레이아웃 align을 맞춰야 합니다. (`alignas(16)` 사용)

[566s](https://youtu.be/wlLGLWI9Fdc?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=566)
![i9_align.jpg](/images/lve/i9_align.jpg)


<br/>

**(2) createPipelineLayout() 에서 VkPushConstantRange 세팅**

<br/>

**(3) recordCommandBuffer() 에서 SimplePushConstantData 에 적절히 값 채워서 vkCmdPushConstants() 호출**
. vkCmdPushConstants() 호출 한 후에 draw()

```cpp
static int frame = 0;  
frame = (frame + 1) % 100;

// ...

for (int j = 0; j < 4; j++) {  
   SimplePushConstantData push{};  
   push.offset = { -0.5f + frame * 0.02f, -0.4f + j * 0.25f };  
   push.color = { 0.0f, 0.0f, 0.2f + 0.2f * j };  
  
   vkCmdPushConstants(  
      commandBuffers[imageIndex],  
      pipelineLayout,  
      VK_SHADER_STAGE_VERTEX_BIT | VK_SHADER_STAGE_FRAGMENT_BIT,  
      0,  
      sizeof(SimplePushConstantData),  
      &push);  
  
   lveModel->draw(commandBuffers[imageIndex]);  
}
```

<br/>


### 3.2. simple_shader.vert

```glsl
// ...
layout(push_constant) uniform Push {  // 요렇게 push constant 받는 거 명시
 vec2 offset;  
 vec3 color;  
} push;

void main() {  
 gl_Position = vec4(position + push.offset, 0.0, 1.0);  // offset 값 사용
}
```

<br/>

### 3.3. simple_shader.frag

```glsl
layout (location = 0) out vec4 outColor;

layout(push_constant) uniform Push {  // 여기서도 요렇게 받음
 vec2 offset;  
 vec3 color;  
} push;

void main() {  
 outColor = vec4(push.color, 1.0);  // color 값 사용
}
```

<br/>

### 3.4. 결과 화면

![i9_result.png](/images/lve/i9_result.png)
요 삼각형들이 오른쪽으로 움직입니다.


<br/>

### 3.5. 다이어그램
![i9_diagram.png](/images/lve/i9_diagram.png)


<br/>

---


## 4. Summary
[730s](https://youtu.be/wlLGLWI9Fdc?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=730)
![i9_summary.jpg](/images/lve/i9_summary.jpg)

해당 모델의 draw call 전에 불려야 해서, 모델 여러개를 Vertex Buffer 하나로 바인드 했을 경우에는 사용이 불가합니다.

![i9_summary2.jpg](/images/lve/i9_summary2.jpg)


<br/>

---