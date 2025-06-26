## Case Description
This is a text-to-speech case implemented based on AI basic voice services.

## Implementation Steps:
### 1. Import Necessary Modules
Import the `textToSpeech` and `promptAction` modules, which are used for text-to-speech and displaying prompt messages respectively.
```typescript
import { textToSpeech } from '@kit.CoreSpeechKit'
import { promptAction } from '@kit.ArkUI'
```
Explanation: The `textToSpeech` module provides the text-to-speech function, and the `promptAction` module is used to display prompt messages on the interface.

### 2. Define the Component
Use the `@Entry` and `@ComponentV2` decorators to define a component named `CoreSpeechKit`.
```typescript
@Entry
@ComponentV2
struct CoreSpeechKit {
  // Text content to be broadcast
  text: string = `
      In March at 45 degrees north latitude, time ties a gentle knot here. The morning mist by the Songhua River wraps fine snow, carving each branch into transparent glass. Ice crystals stack on the branches to form magnolias with a thousand petals. What rustles down when the wind passes through the branches is not snowflakes, but clearly the fragments of stars falling into the world.
      At this time, the apricot blossoms in Jiangnan are brewing honey against the warm wind. The pink clouds of flower shadows spread over the white walls and black tiles, and even the air is permeated with a slightly intoxicating sweetness. The colorist of the seasons quietly divides the palette in half - the north is still persistently continuing the end of winter, using icicles as a pen to outline frost flowers on the window panes; the south is already eager to open the first page of spring, letting the swallows fly over the ink - painted alleys with the willow color.
      This temperature difference of three thousand miles weaves two silk brocades at both ends of the twilight line: the north is a plain brocade embroidered with silver threads, wrapped with unspoken whispers in the cold; the south is a soft silk dyed by silk - reeling women, rippling with the newborn whispers in the warmth. When the rime in Harbin melts the first drop of brilliance in the morning sun, the magnolias in Hangzhou just shake off the third piece of moonlight on their shoulders. It turns out that spring is stepping on the latitude and longitude lines, composing the most moving polyphony between the harshness and the warmth in the world.
  `
  // Text-to-speech engine instance
  ttsEngine?: textToSpeech.TextToSpeechEngine

  // Local state to mark whether it is playing
  @Local isPlaying: boolean = false

  // Initialize the text-to-speech engine
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

  // Called when the component is about to appear
  aboutToAppear(): void {
    this.initTextToSpeechEngine()
  }

  // Called when the component is about to disappear
  aboutToDisappear(): void {
    if (canIUse('SystemCapability.AI.TextToSpeech')) {
      this.ttsEngine?.stop()
      this.ttsEngine?.shutdown()
    }
  }

  // Play voice
  play() {
    if (canIUse('SystemCapability.AI.TextToSpeech')) {
      if (this.ttsEngine?.isBusy()) {
        return promptAction.showToast({ message: 'Playing...' })
      }
      const params: textToSpeech.SpeakParams = {
        requestId: '10000'
      }
      this.ttsEngine?.speak(this.text, params)
      this.isPlaying = true
    }
  }

  // Build the component interface
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
Explanation:
- `text` stores the text content to be broadcast.
- `ttsEngine` is an instance of the text-to-speech engine.
- `isPlaying` is used to mark whether the voice is being played.
- The `initTextToSpeechEngine` method is used to initialize the text-to-speech engine.
- The `aboutToAppear` method is called when the component is about to appear to initialize the engine.
- The `aboutToDisappear` method is called when the component is about to disappear to stop and shut down the engine.
- The `play` method is used to play the voice. If the engine is busy, it will display a prompt message.
- The `build` method builds the component interface, including a list for displaying text and a play button.

## Summary:
### Key Points
- Use the `textToSpeech` module to implement the text-to-speech function.
- Initialize and shut down the engine in the component lifecycle methods.
- Control the display of the play button through the `isPlaying` state.

### Complete Code
```typescript
import { textToSpeech } from '@kit.CoreSpeechKit'
import { promptAction } from '@kit.ArkUI'

@Entry
@ComponentV2
struct CoreSpeechKit {
  text: string = `
      In March at 45 degrees north latitude, time ties a gentle knot here. The morning mist by the Songhua River wraps fine snow, carving each branch into transparent glass. Ice crystals stack on the branches to form magnolias with a thousand petals. What rustles down when the wind passes through the branches is not snowflakes, but clearly the fragments of stars falling into the world.
      At this time, the apricot blossoms in Jiangnan are brewing honey against the warm wind. The pink clouds of flower shadows spread over the white walls and black tiles, and even the air is permeated with a slightly intoxicating sweetness. The colorist of the seasons quietly divides the palette in half - the north is still persistently continuing the end of winter, using icicles as a pen to outline frost flowers on the window panes; the south is already eager to open the first page of spring, letting the swallows fly over the ink - painted alleys with the willow color.
      This temperature difference of three thousand miles weaves two silk brocades at both ends of the twilight line: the north is a plain brocade embroidered with silver threads, wrapped with unspoken whispers in the cold; the south is a soft silk dyed by silk - reeling women, rippling with the newborn whispers in the warmth. When the rime in Harbin melts the first drop of brilliance in the morning sun, the magnolias in Hangzhou just shake off the third piece of moonlight on their shoulders. It turns out that spring is stepping on the latitude and longitude lines, composing the most moving polyphony between the harshness and the warmth in the world.
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
        return promptAction.showToast({ message: 'Playing...' })
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