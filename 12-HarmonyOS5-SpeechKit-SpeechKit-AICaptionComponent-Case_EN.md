# Case Study of SpeechKit AICaption Component in HarmonyOS 5.0

## Abstract
This article introduces how to use `@hms.ai.AICaption` and `@kit.AudioKit` to implement voice recording and AI caption display functions in HarmonyOS 5.0. By creating the `SpeechKitAICaptionComponent` component, users can control the start and end of recording and display AI captions during recording.

## Implementation Steps
1. Import relevant modules from `@hms.ai.AICaption` and `@kit.AudioKit`.
2. Define the `SpeechKitAICaptionComponent` component and initialize `AICaptionController` and `AudioCapturer`.
3. Implement the `start` method to initialize the audio capturer, start recording, and write audio data to `AICaptionController`.
4. Implement the `stop` method to release audio capturer resources.
5. Build the UI interface, including start recording and stop recording buttons, and the AI caption component.

## Implementation Code
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
      Button('Start Recording') 
        .onClick(() => { 
          this.start() 
        }) 
      Button('Stop Recording') 
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

## Summary
This case demonstrates how to use `@hms.ai.AICaption` and `@kit.AudioKit` to implement voice recording and AI caption display in HarmonyOS 5.0. The key knowledge points include the initialization and release of the audio capturer, audio data processing, and the use of the AI caption component. These methods can serve as a foundation for expanding more complex voice - related functions.


## Application Scenarios
Speech Kit (Scenario-based Voice Service) integrates voice AI capabilities, including the TextReader and AICaptionComponent. These capabilities facilitate user-device interaction and enable text reading for users. The TextReader has a wide range of applications. For example, when users are unable or inconvenient to read on - screen text, it can read news aloud to provide information. The AICaptionComponent is also widely used. For instance, it can provide caption services when users are unfamiliar with the audio source language or when the audio is muted.

## Constraints
- Supported languages: Chinese and English.
- Supported audio streams: Currently only "pcm" encoding is supported.
- Audio sampling rate: Currently only 16000 Hz is supported.
- Audio channels: Currently only 1 channel is supported.
- Audio sampling bytes: Currently only 16 - bit is supported.
- Some models are not supported yet, including nova 12, nova 12 Pro, nova 13, nova 13 Pro, and nova 14.
- Device restrictions: This Kit is only applicable to Phone, Tablet, and 2 - in - 1 devices.
- Regional restrictions: This Kit only supports services within China.