# HarmonyOS 5.0 中 SpeechKit 文本朗读功能案例

## 内容摘要
本文介绍了在 HarmonyOS 5.0 里如何使用 `@kit.SpeechKit` 实现文本朗读功能。通过创建 `SpeechKitTextReader` 组件，用户可以实现文本的朗读操作，同时能够监听朗读状态的变化。

## 实现步骤
1. 导入 `@kit.SpeechKit` 中的相关模块。
2. 定义 `SpeechKitTextReader` 组件，并设置朗读状态和待朗读的文本列表。
3. 在组件即将显示时初始化文本朗读器，并监听状态变化。
4. 实现朗读方法，用于启动文本朗读。
5. 在组件即将消失时释放文本朗读器资源。
6. 构建 UI 界面，包含朗读图标和待朗读的文本。

## 落地代码
```typescript
import { ReadStateCode, TextReader, TextReaderIcon } from '@kit.SpeechKit' 

@Entry 
@Component 
struct SpeechKitTextReader { 
  @State readState: ReadStateCode = ReadStateCode.WAITING; 
  list: TextReader.ReadInfo[] = [ 
    { 
      id: '10001', 
      title: { text: '三月的东北', isClickable: false }, 
      author: { text: '鸿蒙开发者', isClickable: false }, 
      date: { text: '2025-06-16', isClickable: false }, 
      bodyInfo: '北纬45度的三月，时光在这里打了个温柔的结。松花江畔的晨雾裹着细雪，将每根枝条都雕琢成剔透的琉璃，冰晶在枝头堆叠出千重瓣的玉兰花，风穿过枝桠时簌簌抖落的不是雪粒，分明是星辰坠入人间的碎屑。' 
    } 
  ] 
  item = this.list[0] 

  async aboutToAppear(): Promise<void> { 
    const readerParam: TextReader.ReaderParam = { 
      isVoiceBrandVisible: true, 
      businessBrandInfo: { 
        panelName: '小艺朗读' 
      } 
    } 
    await TextReader.init(getContext(this), readerParam); 
    TextReader.on('stateChange', (state) => { 
      if (this.item.id === state.id) { 
        this.readState = state.state; 
      } else { 
        this.readState = ReadStateCode.WAITING; 
      } 
    }) 
  } 

  async reading() { 
    await TextReader.start(this.list, this.item.id) 
  } 

  aboutToDisappear(): void { 
    TextReader.release() 
  } 

  build() { 
    Column({ space: 15 }) { 
      Row() { 
        Blank() 
        TextReaderIcon({ readState: this.readState }) 
          .onClick(() => { 
            this.reading() 
          }) 
      } 
      .width('100%') 

      Text(this.list[0].bodyInfo) 
    } 
    .padding(15) 
    .height('100%') 
    .width('100%') 
  } 
} 
```

## 总结
本案例展示了在 HarmonyOS 5.0 中使用 `@kit.SpeechKit` 实现文本朗读功能的方法。关键知识点包括组件的生命周期管理（`aboutToAppear` 和 `aboutToDisappear`）、文本朗读器的初始化和释放、朗读状态的监听，以及 UI 组件的构建。这些方法可以作为基础，用于扩展更复杂的语音相关功能。

## 适用场景
Speech Kit（场景化语音服务）集成了语音类AI能力，包括朗读控件（TextReader）和AI字幕控件（AICaptionComponent）能力，便于用户与设备进行互动，为用户实现朗读文章。朗读控件应用广泛，例如在用户不方便或者无法查看屏幕文字时，为用户朗读新闻，提供资讯。AI字幕控件应用广泛，例如在用户不熟悉音频源语言或者静音时，为用户提供字幕服务。

## 约束限制

支持的语种类型：中文。
设备限制：本Kit仅适用于Phone、Tablet、和2in1设备。
地区限制：本Kit仅支持中国提供服务。
