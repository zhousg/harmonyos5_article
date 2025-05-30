## 案例描述
这是一个基于AI基础语音服务实现的文字语音播报案例
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/36a0671fde3647c7a5707f0d7e7f8a7b.png)

## 实现步骤：
  ### 1. 引入必要的模块
  引入 `textToSpeech` 和 `promptAction` 模块，分别用于文字转语音和提示信息展示。
  ```typescript
  import { textToSpeech } from '@kit.CoreSpeechKit'
  import { promptAction } from '@kit.ArkUI'
  ```
  说明：`textToSpeech` 模块提供了文字转语音的功能，`promptAction` 模块用于在界面上显示提示信息。

  ### 2. 定义组件
  使用 `@Entry` 和 `@ComponentV2` 装饰器定义一个名为 `CoreSpeechKit` 的组件。
  ```typescript
  @Entry
  @ComponentV2
  struct CoreSpeechKit {
    // 待播报的文本内容
    text: string = `
        北纬45度的三月，时光在这里打了个温柔的结。松花江畔的晨雾裹着细雪，将每根枝条都雕琢成剔透的琉璃，冰晶在枝头堆叠出千重瓣的玉兰花，风穿过枝桠时簌簌抖落的不是雪粒，分明是星辰坠入人间的碎屑。
        而此时江南的杏花正枕着暖风酿蜜，绯云般的花影漫过白墙黛瓦，连空气都沁着微醺的甜。季节的调色师悄悄把颜料盘一分为二——北国仍执着地续写冬的尾章，用冰棱作笔，在窗棂勾勒霜花；南疆已迫不及待地翻开春的扉页，让雨燕衔着柳色掠过水墨长巷。
        这相隔三千里的温差，在晨昏线两端织就两幅绸缎：北方是银丝绣的素锦，凛冽里裹着未尽的絮语；南方是蚕娘染的软罗，温润中漾开初生的呢喃。当哈尔滨的树挂迎着朝阳融化第一滴璀璨，杭州的玉兰恰好抖落肩头第三片月光，原来春天正踏着经纬线，把料峭与暄暖谱成天地间最动人的复调。
    `
    // 文字转语音引擎实例
    ttsEngine?: textToSpeech.TextToSpeechEngine

    // 本地状态，用于标记是否正在播放
    @Local isPlaying: boolean = false

    // 初始化文字转语音引擎
    async initTextToSpeechEngine() {
      if (canIUse('SystemCapability.AI.TextToSpeech')) {
        const params: textToSpeech.CreateEngineParams = {
          language: 'zh-CN',
          person: 0,
          online: 1
        }
        this.ttsEngine = await textToSpeech.createEngine(params)
      }
    }

    // 组件即将显示时调用
    aboutToAppear(): void {
      this.initTextToSpeechEngine()
    }

    // 组件即将消失时调用
    aboutToDisappear(): void {
      if (canIUse('SystemCapability.AI.TextToSpeech')) {
        this.ttsEngine?.stop()
        this.ttsEngine?.shutdown()
      }
    }

    // 播放语音
    play() {
      if (canIUse('SystemCapability.AI.TextToSpeech')) {
        if (this.ttsEngine?.isBusy()) {
          return promptAction.showToast({ message: '正在播报...' })
        }
        const params: textToSpeech.SpeakParams = {
          requestId: '10000'
        }
        this.ttsEngine?.speak(this.text, params)
        this.isPlaying = true
      }
    }

    // 构建组件界面
    build() {
      Stack({ alignContent: Alignment.BottomEnd }) {
        List() {
          ListItem() {
            Text(this.text)
              .lineHeight(32)
          }
        }
        .padding({ left: 15, right: 15 })
        .height('100%')
        .width('100%')

        Row() {
          Image(this.isPlaying ? $r('sys.media.AI_playing') : $r('sys.media.AI_play'))
            .width(24)
            .onClick(() => this.play())
        }
        .borderRadius(24)
        .shadow({
          color: Color.Gray,
          offsetX: 5,
          offsetY: 5,
          radius: 15
        })
        .width(48)
        .aspectRatio(1)
        .justifyContent(FlexAlign.Center)
        .margin({ right: 50, bottom: 50 })
      }
      .height('100%')
      .width('100%')
    }
  }
  ```
  说明：
  - `text` 存储待播报的文本内容。
  - `ttsEngine` 是文字转语音引擎的实例。
  - `isPlaying` 用于标记是否正在播放语音。
  - `initTextToSpeechEngine` 方法用于初始化文字转语音引擎。
  - `aboutToAppear` 方法在组件即将显示时调用，初始化引擎。
  - `aboutToDisappear` 方法在组件即将消失时调用，停止并关闭引擎。
  - `play` 方法用于播放语音，若引擎正在忙碌则显示提示信息。
  - `build` 方法构建组件的界面，包括显示文本的列表和播放按钮。

## 总结梳理：
  ### 核心点
  - 使用 `textToSpeech` 模块实现文字转语音功能。
  - 在组件生命周期方法中初始化和关闭引擎。
  - 通过 `isPlaying` 状态控制播放按钮的显示。

  ### 完整代码
  ```typescript
  import { textToSpeech } from '@kit.CoreSpeechKit'
  import { promptAction } from '@kit.ArkUI'

  @Entry
  @ComponentV2
  struct CoreSpeechKit {
    text: string = `
        北纬45度的三月，时光在这里打了个温柔的结。松花江畔的晨雾裹着细雪，将每根枝条都雕琢成剔透的琉璃，冰晶在枝头堆叠出千重瓣的玉兰花，风穿过枝桠时簌簌抖落的不是雪粒，分明是星辰坠入人间的碎屑。
        而此时江南的杏花正枕着暖风酿蜜，绯云般的花影漫过白墙黛瓦，连空气都沁着微醺的甜。季节的调色师悄悄把颜料盘一分为二——北国仍执着地续写冬的尾章，用冰棱作笔，在窗棂勾勒霜花；南疆已迫不及待地翻开春的扉页，让雨燕衔着柳色掠过水墨长巷。
        这相隔三千里的温差，在晨昏线两端织就两幅绸缎：北方是银丝绣的素锦，凛冽里裹着未尽的絮语；南方是蚕娘染的软罗，温润中漾开初生的呢喃。当哈尔滨的树挂迎着朝阳融化第一滴璀璨，杭州的玉兰恰好抖落肩头第三片月光，原来春天正踏着经纬线，把料峭与暄暖谱成天地间最动人的复调。
    `
    ttsEngine?: textToSpeech.TextToSpeechEngine

    @Local isPlaying: boolean = false

    async initTextToSpeechEngine() {
      if (canIUse('SystemCapability.AI.TextToSpeech')) {
        const params: textToSpeech.CreateEngineParams = {
          language: 'zh-CN',
          person: 0,
          online: 1
        }
        this.ttsEngine = await textToSpeech.createEngine(params)
      }
    }

    aboutToAppear(): void {
      this.initTextToSpeechEngine()
    }

    aboutToDisappear(): void {
      if (canIUse('SystemCapability.AI.TextToSpeech')) {
        this.ttsEngine?.stop()
        this.ttsEngine?.shutdown()
      }
    }

    play() {
      if (canIUse('SystemCapability.AI.TextToSpeech')) {
        if (this.ttsEngine?.isBusy()) {
          return promptAction.showToast({ message: '正在播报...' })
        }
        const params: textToSpeech.SpeakParams = {
          requestId: '10000'
        }
        this.ttsEngine?.speak(this.text, params)
        this.isPlaying = true
      }
    }

    build() {
      Stack({ alignContent: Alignment.BottomEnd }) {
        List() {
          ListItem() {
            Text(this.text)
              .lineHeight(32)
          }
        }
        .padding({ left: 15, right: 15 })
        .height('100%')
        .width('100%')

        Row() {
          Image(this.isPlaying ? $r('sys.media.AI_playing') : $r('sys.media.AI_play'))
            .width(24)
            .onClick(() => this.play())
        }
        .borderRadius(24)
        .shadow({
          color: Color.Gray,
          offsetX: 5,
          offsetY: 5,
          radius: 15
        })
        .width(48)
        .aspectRatio(1)
        .justifyContent(FlexAlign.Center)
        .margin({ right: 50, bottom: 50 })
      }
      .height('100%')
      .width('100%')
    }
  }
  ```
