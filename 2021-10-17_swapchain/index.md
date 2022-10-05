# 'Swap Chain Overview - Vulkan Game Engine Tutorial 05 part 1' 정리


[Youtube Link](https://www.youtube.com/watch?v=IUYH74MqxOA&list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR)

{{< youtube IUYH74MqxOA >}}


## 1. 주요 내용

- SwapChain 관련 소스 다운 받아서 적용 (이해는 천천히)
- 고정 댓글에 PipelineConfigInfo 관련 [추가 수정](https://github.com/blurrypiano/littleVulkanEngine/commit/a867ab39c43ccc89ca744db84137df179b41daa7) 있음
- Swap Chain, V-Sync, Presentation Mode, Render Pass 설명

## 2. Swap Chain

Swap Chain is a series of framebuffer.

[8s](https://www.youtube.com/watch?v=IUYH74MqxOA&list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=8)  

![swapchain](/images/lve/i5_swapchain.jpg)


Framebuffer 여러 개를 들고 있는 게 Swap Chain 이라고 생각하면 되는 듯 합니다.  

그래픽 파이프라인을 쭉 지나면서 FrameBuffer 를 채우는데, 어떤 Framebuffer를 써야 하는지를 Swap Chain이 관리합니다.  

[17s](https://www.youtube.com/watch?v=IUYH74MqxOA&list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=17) 

![swapchain2](/images/lve/i5_swapchain2.jpg)


어떤 Framebuffer를 실제 Surface에 보여줄지도 Swap Chain 이 관리합니다.  

Framebuffer는 보통 Color Buffer와 Depth Buffer를 가지고 있습니다. (Attachment 라고 부릅니다.)  

요 중에 Color Buffer를 Surface 그릴 때 사용하는 듯 합니다.  

아마도 OS가 '이번에 그릴 Buffer 주렴' 하면, 적절한 Framebuffer의 Color Buffer를 던져 줄 듯 합니다.  

[41s](https://www.youtube.com/watch?v=IUYH74MqxOA&list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=41)

![double buffering](/images/lve/i5_double_buffering.jpg)


대부분의 OS는 Framebuffer를 두 개 사용하는 더블 버퍼링을 하고 있습니다.  

지금 display 되는 Framebuffer를 Front Buffer 라고 하고,  

지금 그래픽 파이프라인이 열심히 그리고 있는 Framebuffer를 Back Buffer 라고 부릅니다.  

Swap Chain 이 요 Framebuffer들의 싱크 맞추는 것도 담당합니다.  

## 3. V-Sync

[66s](https://www.youtube.com/watch?v=IUYH74MqxOA&list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=66)

![v-sync](/images/lve/i5_v-sync.jpg)

싱크의 기본이 되는 것이 V-Sync(화면을 위쪽부터 한 줄씩 다 그린 후, 준비를 위해 화면 아래 한 줄을 더 가는 느낌) 입니다.  

디스플레이 장치의 Refresh Rate가 60Hz인 경우,  

V-Sync 신호가 1/60초(16.6ms) 마다 발생합니다.  

요 V-Sync 신호에 front buffer 와 back buffer 를 스왑합니다.  

[106s](https://www.youtube.com/watch?v=IUYH74MqxOA&list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=106)

![i5_v-sync_waste.jpg](/images/lve/i5_v-sync_waste.jpg)


요 V-Sync 에 맞춰 렌더링을 하면 빨간색으로 칠한 시간 동안은 gpu가 놀고 있는 상태입니다.  

[133s](https://www.youtube.com/watch?v=IUYH74MqxOA&list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=133)  

![i5_v-sync_waste2.jpg](/images/lve/i5_v-sync_waste2.jpg)


다음 V-Sync 가 왔는데도, gpu 가 아직 일을 다 끝내지 못했다면,  

아직 back buffer가 준비되지 않았으므로, swap 을 하지 못하고 같은 화면이 다음 frame에도 보이게 됩니다.  

요게 많아지면 사용자 입장에서 화면이 프레임이 뚝뚝 끊겨 보이게 됩니다.  

[156s](https://www.youtube.com/watch?v=IUYH74MqxOA&list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=156)

![i5_threeple_buffering.jpg](/images/lve/i5_threeple_buffering.jpg)

framebuffer를 하나 더 사용해서 tripple buffering을 하면 요 상황을 개선할 수 있습니다.  

다만 메모리를 framebuffer 하나만큼 더 사용하겠지요.  

OpenGL 에서는 tripple buffering 관련한 세팅을 별로 신경 안써도 됐는데, Vulakn에서는 명시적으로 해줘야 한다고 합니다.  


<br/>

**지금 시점에서 알아둬야 할 것**

> - We will have multiple framebuffers (2 or 3)
> - We can use swapChain.acquireNextImage() to get the next index of our framebuffers

## 4. Presentation Mode

[492s](https://youtu.be/IUYH74MqxOA?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=492)  

![i5_presentation_mode.jpg](/images/lve/i5_presentation_mode.jpg)

요 sync 관련 동작을 어떻게 할 것이냐에를 정하기 위해 몇 가지 모드가 있습니다.  

[529s](https://youtu.be/IUYH74MqxOA?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=529)  

![i5_presentation_mode_comparison.jpg](/images/lve/i5_presentation_mode_comparison.jpg)
 

일단 FIFO (V-Sync 사용) 모드로 진행합니다.  

모바일에서는 소모 전류 이슈로 인해서 Mainbox나 Immediate를 쓸 일은 없어 보입니다.  

## 5. Render Pass

[883s](https://www.youtube.com/watch?v=IUYH74MqxOA&list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR&t=883)  

![i5_render_pass.jpg](/images/lve/i5_render_pass.jpg)


Render Pass 는 Framebuffer의 구조와 포맷에 관한 정보를 가지고 있습니다. (어떤 attachment 들을 갖고 있는지)  

그래픽 파이프라인에게 '너가 채워야할 Framebuffer는 요렇게 생겼단다.' 하고 알려주는 blueprint 느낌입니다.  

## 6. FirstApp 클래스 구현

![i5_implementation1.png](/images/lve/i5_implementation1.png)

- lveSwapChain 멤버 추가
- lvePipeline 에 스마트 포인터 사용
  - 종종 [https://www.learncpp.com/](https://www.learncpp.com/) 를 읽어 보는 걸 추천하고 있습니다.
- pipelineLayout 멤버 추가
- commandBuffers 멤버 추가


![i5_implementation2.png](/images/lve/i5_implementation2.png)

- FirstApp 생성자 및 private 메소드들 추가
  - createPipelineLayout() : pipelineLayout 만듬 ([vkCreatePipelineLayout()](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/man/html/vkCreatePipelineLayout.html#_parameters))
    - SetLayout : 쉐이더에 텍스처나 유니폼 버퍼 같은 애들 넘기는 용도
    - PushConstant : 아주 작은 데이터를 쉐이더에 넘기는 용도
  - createPipeline() : LvePipeline 생성
  - createCommandBuffers() : 빈 함수
