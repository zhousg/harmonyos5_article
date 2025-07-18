# HarmonyOS 5.0 中 SpeechKit AICaption 组件案例

## 内容摘要
本文介绍了在 HarmonyOS 5.0 里如何使用 `@hms.ai.AICaption` 和 `@kit.AudioKit` 实现语音录音并进行 AI 字幕显示的功能。通过创建 `SpeechKitAICaptionComponent` 组件，用户可以控制录音的开始和结束，并在录音时显示 AI 字幕。

## 实现步骤
1. 导入 `@hms.ai.AICaption` 和 `@kit.AudioKit` 中的相关模块。
2. 定义 `SpeechKitAICaptionComponent` 组件，初始化 `AICaptionController` 和 `AudioCapturer`。
3. 实现 `start` 方法，用于初始化音频捕获器并开始录音，将音频数据写入 `AICaptionController`。
4. 实现 `stop` 方法，用于释放音频捕获器资源。
5. 构建 UI 界面，包含开始录音和结束录音按钮，以及 AI 字幕组件。

## 落地代码
```typescript
import { AICaptionComponent, AICaptionController, AudioData } from '@hms.ai.AICaption'; 
import { audio } from '@kit.AudioKit'; 

@Entry 
@Component 
struct SpeechKitAICaptionComponent { 

  controller = new AICaptionController 

  audioCapturer?:  audio.AudioCapturer 

  @State isShown: boolean = false 

  async start () { 
    let audioStreamInfo: audio.AudioStreamInfo = { 
      samplingRate: audio.AudioSamplingRate.SAMPLE_RATE_16000, 
      channels: audio.AudioChannel.CHANNEL_1, 
      sampleFormat: audio.AudioSampleFormat.SAMPLE_FORMAT_S16LE, 
      encodingType: audio.AudioEncodingType.ENCODING_TYPE_RAW 
    }; 

    let audioCapturerInfo: audio.AudioCapturerInfo = { 
      source: audio.SourceType.SOURCE_TYPE_MIC, 
      capturerFlags: 0 
    }; 

    let audioCapturerOptions: audio.AudioCapturerOptions = { 
      streamInfo: audioStreamInfo, 
      capturerInfo: audioCapturerInfo 
    }; 

    this.audioCapturer = await audio.createAudioCapturer(audioCapturerOptions) 
    this.audioCapturer.on('readData', (buffer) => { 
      const audioData: AudioData = { data: new Uint8Array(buffer) } 
      this.controller.writeAudio(audioData) 
    }) 
    await this.audioCapturer.start() 
    this.isShown = true 
  } 

  async stop () { 
    await this.audioCapturer?.release() 
    this.isShown = false 
  } 

  build() { 
    Column({ space: 15 }) { 
      Button('开始录音') 
        .onClick(() => { 
          this.start() 
        }) 
      Button('结束录音') 
        .onClick(() => { 
          this.stop() 
        }) 
      if (this.isShown) { 
        AICaptionComponent({ 
          isShown: this.isShown, 
          controller: this.controller, 
          options: { 
            initialOpacity: 1, 
            onPrepared: () => { 
              // 
            }, 
            onError: (e) => { 
              // 
            } 
          } 
        }) 
          .width('100%') 
          .height(100) 
      } 
    } 
    .padding(15) 
    .height('100%') 
    .width('100%') 
  } 
} 
```

## 总结
本案例展示了在 HarmonyOS 5.0 中使用 `@hms.ai.AICaption` 和 `@kit.AudioKit` 实现语音录音并显示 AI 字幕的方法。关键知识点包括音频捕获器的初始化和释放、音频数据的处理，以及 AI 字幕组件的使用。这些方法可以作为基础，用于扩展更复杂的语音相关功能。

## 适用场景
Speech Kit（场景化语音服务）集成了语音类AI能力，包括朗读控件（TextReader）和AI字幕控件（AICaptionComponent）能力，便于用户与设备进行互动，为用户实现朗读文章。朗读控件应用广泛，例如在用户不方便或者无法查看屏幕文字时，为用户朗读新闻，提供资讯。AI字幕控件应用广泛，例如在用户不熟悉音频源语言或者静音时，为用户提供字幕服务。

## 约束限制

支持的语种类型：中英文。
支持的音频流：音频类型：当前仅支持 "pcm"编码。
音频采样率：当前仅支持16000采样率。
音频声道：当前仅支持1个通道。
音频采样字节：当前仅支持16位。
部分机型暂不支持，包括nova 12/nova 12 Pro/nova 13/nova 13 Pro/nova 14。
设备限制：本Kit仅适用于Phone、Tablet、和2in1设备。
地区限制：本Kit仅支持中国提供服务。