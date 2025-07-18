# Case Study of SpeechKit Text Reading Function in HarmonyOS 5.0

## Abstract
This article introduces how to use `@kit.SpeechKit` to implement the text reading function in HarmonyOS 5.0. By creating the `SpeechKitTextReader` component, users can perform text reading operations and monitor changes in the reading status.

## Implementation Steps
1. Import relevant modules from `@kit.SpeechKit`.
2. Define the `SpeechKitTextReader` component and set the reading status and the list of texts to be read.
3. Initialize the text reader when the component is about to appear and monitor status changes.
4. Implement the reading method to start text reading.
5. Release the text reader resources when the component is about to disappear.
6. Build the UI interface, including the reading icon and the text to be read.

## Implementation Code
```typescript
import { ReadStateCode, TextReader, TextReaderIcon } from '@kit.SpeechKit' 

@Entry 
@Component 
struct SpeechKitTextReader { 
  @State readState: ReadStateCode = ReadStateCode.WAITING; 
  list: TextReader.ReadInfo[] = [ 
    { 
      id: '10001', 
      title: { text: 'Northeast China in March', isClickable: false }, 
      author: { text: 'HarmonyOS Developer', isClickable: false }, 
      date: { text: '2025-06-16', isClickable: false }, 
      bodyInfo: 'In March at 45 degrees north latitude, time ties a gentle knot here. The morning mist by the Songhua River wraps fine snow, carving each branch into transparent glass. Ice crystals stack on the branches to form thousand - petaled magnolias. What rustles down when the wind passes through the branches is not snowflakes, but clearly the debris of stars falling into the world.' 
    } 
  ] 
  item = this.list[0] 

  async aboutToAppear(): Promise<void> { 
    const readerParam: TextReader.ReaderParam = { 
      isVoiceBrandVisible: true, 
      businessBrandInfo: { 
        panelName: 'XiaoYi Reading' 
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

## Summary
This case demonstrates how to use `@kit.SpeechKit` to implement the text reading function in HarmonyOS 5.0. The key knowledge points include component lifecycle management (`aboutToAppear` and `aboutToDisappear`), initialization and release of the text reader, monitoring of the reading status, and construction of UI components. These methods can serve as a foundation for expanding more complex voice - related functions.


## Application Scenarios
Speech Kit (Scenario-based Voice Service) integrates voice AI capabilities, including the TextReader control and the AICaptionComponent control. These capabilities facilitate user-device interaction and enable text reading. The TextReader control has a wide range of applications. For example, when users are unable to view screen text, it can read news aloud to provide information. The AICaptionComponent control is also widely used. For instance, when users are unfamiliar with the audio source language or have the device on mute, it can provide subtitles.

## Constraints
- Supported Languages: Chinese.
- Device Restrictions: This Kit is only applicable to Phone, Tablet, and 2-in-1 devices.
- Region Restrictions: This Kit only provides services within China (excluding Hong Kong, Macau, and Taiwan of China).
