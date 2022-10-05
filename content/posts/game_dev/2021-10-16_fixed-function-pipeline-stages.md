---
title: "'Fixed Function Pipeline Stages - Vulkan Game Engine Tutorial 04' 정리"
date: 2021-10-16T23:30:00+09:00
summary: "graphics pipeline을 생성할 준비"
categories: [ 게임 개발 ]
series: [ Vulkan Game Engine Tutorial 정리 ]
series_weight: 5
# featuredImage: /images/lve/i5-2_cover.jpg
tags:
- vulkan
---

[Youtube Link](https://www.youtube.com/watch?v=ecMcXW6MSYU&list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR)

{{< youtube ecMcXW6MSYU >}}

​
## 1. 주요 내용
​
- `PipelineConfigInfo` 구조체를 하나씩 채움
- [`vkCreateGraphicsPipelines()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkCreateGraphicsPipelines.html) 호출해서 파이프라인을 생성할 준비
​
---
​
## 2. 실습 코드 수정
​
이번에 하고 싶은건 그래픽 파이프라인(`VkPipeline`)을 하나 만드는 것입니다.
​
  
[`vkCreateGraphicsPipelines()`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkCreateGraphicsPipelines.html) 함수를 호출하고 싶은 건데,  
만들어진 파이프라인이 어떻게 동작할 건지 설정 값들을 미리 다 세팅해줘야 합니다.
​

이를 위해 [VkGraphicsPipelineCreateInfo](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/VkGraphicsPipelineCreateInfo.html) 구조체를 채워야 합니다.  
​
['Graphics Pipeline Overview - Vulkan Game Engine Tutorial 02' 정리](https://mechurak.tistory.com/76?category=929034) 에서 봤던 그래픽 파이프라인을 다시 한 번 봐봅시다.
​
![triangle](/images/lve/i2_graphics_pipeline.jpg)

​
[`VkGraphicsPipelineCreateInfo`](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/VkGraphicsPipelineCreateInfo.html) 구조체가 각 단계별 동작의 세팅 값 및 부가적으로 필요한 정보를 가지고 있습니다. 요걸 채워서 함수 호출할 때 넘겨줘야 합니다.  
거대한 구조체네요ㅠㅜ  ​

위 구조체에서 일부 세팅 값들을 `PipelineConfigInfo`로 따로 빼두었습니다.  
아마도 일부 세팅 값을 바꿔서 비슷한 그래픽 파이프라인을 또 만들 때 사용할 예정인 듯 합니다.

​

### 2.1. inputAssemblyInfo
​
맨 처음 들어온 데이터 뭉치를 어떻게 해석할지 정합니다.  
​세 개씩 끊어서 삼각형 하나로 볼지,  ​
두 번째 그림처럼 중첩해서 삼각형을 만들지 정합니다.  

​
참고로, 인풋 데이터는 위치 정보외에 다른 정보들도 포함될 수 있습니다.
​

[79s](https://youtu.be/ecMcXW6MSYU?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=79s)  

![input assembly](/images/lve/i4_input_assembly.jpg)
​


[124s](https://youtu.be/ecMcXW6MSYU?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=124s)  
​
![triangle strip](/images/lve/i4_triangle_strip.jpg)

​
이를 위해, [VkPipelineInputAssemblyStateCreateInfo](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/VkPipelineInputAssemblyStateCreateInfo.html) 를 세팅합니다.
​


### 2.2. viewportInfo
​
Vertext Shader 까지는 \[-1, -1\] ~ \[1, 1\] 좌표계(NDC, Normalized Device Coordinates)를 사용합니다.
​
요 좌표계를 가져다가 fragmentbuffer 의 어느 부분에 그릴 것인지를 정해야 합니다.
​
이를 위해, [VkPipelineViewportStateCreateInfo](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/VkPipelineViewportStateCreateInfo.html) 를 세팅합니다.
​
요 안에 viewport와 scissor 가 또 있습니다.

[209s](https://youtu.be/ecMcXW6MSYU?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=209s)  

![viewport](/images/lve/i4_view_port.jpg)

​
위 그림에서 어떻게 매핑할 것인지가 viewport 설정입니다.  
​
NDC의 (-1, -1), (1,1)을 framebuffer 의 어디로 매핑할 것인지를 결정합니다.  
framebuffer 중에서 특정 영역만 필요한 상황이 있을 수 있습니다. (다른 창에 의해서 가려지거나 해서 그릴 필요가 없는경우?)  
​

이를 위해 scissor가 있습니다.  ​
framebuffer의 특정 영역을 골라서, 그 부분만 그리면 된다고 알려 줍니다.  ​
아직은 빌드가 제대로 안되고,  ​
Tutorial 5-1 까지 구현을 마치고 난 후에, 값을 바꿔서 테스트 해 봅시다.  
​
![viewport and scissor](/images/lve/i4_viewport_scissor.png)
​
(1) 번은 Tutorial 5-1 까지의 결과물 입니다.  
​빨간색이 viewport, 파란색이 scissor 입니다.  
​(2)번은 다음과 같이 viewport를 세팅했습니다.  
​
```cpp
// 시작점 NDC(-1,-1) -> (0, 300)
configInfo.viewport.x = 0.0f;
configInfo.viewport.y = 300.0f;
​
// offset (0, 300) 기준으로 (+400, +300) 위치로 NDC(1, 1)을 매핑
configInfo.viewport.width = static_cast<float>(width / 2);  // 400 
configInfo.viewport.height = static_cast<float>(height / 2);  // 300
configInfo.viewport.minDepth = 0.0f;
configInfo.viewport.maxDepth = 1.0f;
```
​
(3)번은 scissor 까지 세팅했습니다.  
​
```cpp
// offset (0,0) 정하고, 거기에서 (+200, +800) 위치 까지만 표시
configInfo.scissor.offset = { 0, 0 };
configInfo.scissor.extent = { width / 4, height };  // 200, 300
```
​
좋은 그래픽 카드에서는 viewport, scissor 여러 개일 수도 있다고 합니다.  
​
### 2.3. rasterizationInfo
​
그래픽 파이프라인에서 Raterization 은 vertex shader 가 넘겨 준 geometry 데이터들을 받아서  
​fragment 들로 바꿉니다.  
​

위에서 봤던 scissor 도 적용하고(scissor test), 카메라에서 너무 가깝거나 먼지도 확인하고(depth test), 삼각형의 앞면 뒷면 여부도 판단(face culling)도 여기서 합니다.  
​
요 동작들을 위한 세팅을 [VkPipelineRasterizationStateCreateInfo](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/VkPipelineRasterizationStateCreateInfo.html) 에다가 합니다.  
​
이중에 cullMode, frontFace는 삼각형의 뒷면을 런더링 하지 않기 위한 세팅입니다. (face culling)  
​
카메라에 삼각형의 뒷면이 보이는 경우에는 그리지 않음으로써, 성능 향상을 꾀하기 위함입니다.  
​

[391s](https://youtu.be/ecMcXW6MSYU?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=391s)

![cull mode](/images/lve/i4_cull_mode.jpg)
​

​
어떤 방향을 앞면으로 볼 건지 결정할 수 있습니다.


### 2.4. multisampleInfo
​
MSAA: Multisampling Anti-aliasing  
​
Rasterization 과정에서 런더링 해야 하는 fragment 를 선정할 때,  
​각 fragment의 중앙의 점이 삼각형에 포함되는지 여부로 판단하는 듯 합니다.  
​삼각형 안쪽인지 바깥쪽인지가 true / false 로 정해지는 것이죠.  
​
이로 인해, 경계선 부분이 거칠게 보이는 계단 현상(aliasing)이 나타납니다.  
​

[478s](https://youtu.be/ecMcXW6MSYU?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=478s)

![msaa no](/images/lve/i4_msaa_no.jpg)
​
이를 해결하기 위해, Multisample Anti-aliasing을 사용할 수 있습니다.  
​
fragment 를 선정할 때, 한 개 점이 아니라 여러 점을 사용해서, 몇 개의 점이 삼각형에 포함되는지에 따라, 색을 살짝 연하게 칠하던가 하는 듯 합니다.  

​
[488s](https://youtu.be/ecMcXW6MSYU?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=488s)

![msaa yes](/images/lve/i4_msaa_yes.jpg)
​

이를 위해, [VkPipelineMultisampleStateCreateInfo](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/VkPipelineMultisampleStateCreateInfo.html) 를 세팅합니다.
​
### 2.5. colorBlendInfo
​
모델 여러 개가 겹쳐 있는 경우, 특정 fragment (픽셀)을 생각해 봅시다.  
​첫 번째 모델을 그리면, fragment shader 가 색을 결정해서 framebuffer를 채워 놓은 상태일 겁니다.  
​여기서 다음 모델을 그리면, fragment shader 가 framebuffer를 채울 때, 해당 fragment에 기존에 채워져 있는 색깔이랑 같이 잘 표현해야 합니다.  
​
보통은 투명도에 따라 기존 색과 현재 색을 섞는 알파 블렌딩을 사용한다고 합니다.  
​
이를 위해, [VkPipelineColorBlendStateCreateInfo](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/VkPipelineColorBlendStateCreateInfo.html) 를 세팅합니다. (내부에 [VkPipelineColorBlendAttachmentState](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/VkPipelineColorBlendAttachmentState.html))
​
### 2.6. depthStencilInfo
​
framebuffer는 보통 color buffer 와 depth 버퍼를 가집니다.  
​
depth 버퍼는 해당 픽셀에서 카메라와 가장 가까이 있는 object의 거리를 저장합니다. (어떤 object인지는 관심 없음)  
​
산이 그려 졌고 (depth 0.5), 이제 구름 depth 0.8에 을 그린다고 하면  
​
산과 겹치는 부분의 픽셀은 산에 가려질 테니(depth 버퍼에 저장되어 있는 값이, 현재 그릴려는 object의 depth 보다 작으므로) 그릴 필요가 없습니다.  
​

[593s](https://youtu.be/ecMcXW6MSYU?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=593s) 

![msaa yes](/images/lve/i4_deptStencil.jpg)
​
이를 위해 [VkPipelineDepthStencilStateCreateInfo](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/VkPipelineDepthStencilStateCreateInfo.html) 를 세팅합니다.  
​
### 2.7. shaderStage
​
앞 장에서 Vertex Shader, Fragment Shader를 작성해서 VkShaderModule 들을 만들었습니다.  
​
요 정보도 파이프라인 생성할 때, 넘겨 줘야 합니다.  
​
이를 위해, [VkPipelineShaderStateCreateInfo](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/VkPipelineShaderStageCreateInfo.html) 를 세팅합니다.  


​
### 2.8. vertexInputInfo
​
그래픽 파이프라인에 맨 처음 들어오는 데이터 뭉치는 각 Vertex 별로 위치 뿐만 아니라 다른 여러 데이터를 포함할 수 있습니다.  
​
요 데이터들이 어떻게 정렬되어 있는지,  
​
인덱스 버퍼라는 게 따로 있는지 정보도 넘겨줘야 합니다.  
​
이를 위해, [VkPipelineVertexInputStateCreateInfo](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/VkPipelineVertexInputStateCreateInfo.html) 를 세팅합니다.


​
### 2.9. pipelineInfo 대통합

이제 지금까지 준비한 세팅 값들로 [VkGraphicsPipelineCreateInfo](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/VkGraphicsPipelineCreateInfo.html) 구조체를 채웁니다.  
​요 구조체를 넘겨서, [vkCreateGraphicsPipelines()](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkCreateGraphicsPipelines.html) 함수를 드디어 호출합니다.  
(pipelineLayout, renderPass 를 아직 채우지 않아서, 정상 실행은 되지 않습니다.)  
​

---
​
# 3. 다이어그램
​
![클래스 다이어그램](/images/lve/i4_class-diagram.png)
