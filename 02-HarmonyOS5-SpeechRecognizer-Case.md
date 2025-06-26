## Case Description
This is a real-time speech-to-text case implemented based on AI basic voice services. It collects audio through a microphone and converts it into text in real-time.

## Implementation Steps:
### 1. Import Necessary Modules
```typescript
import { speechRecognizer } from '@kit.CoreSpeechKit'
import { abilityAccessCtrl } from '@kit.AbilityKit'
import { promptAction } from '@kit.ArkUI'
```

### 2. Request Microphone Permissions
```typescript
async requestPermissions() {
  const atManager = abilityAccessCtrl.createAtManager();
  const res = await atManager.requestPermissionsFromUser(getContext(), ['ohos.permission.MICROPHONE'])
  this.hasPermissions =
    res.authResults.every(grantStatus => grantStatus === abilityAccessCtrl.GrantStatus.PERMISSION_GRANTED)
}
```

### 3. Initialize the Speech Recognition Engine
```typescript
async startRecord() {
  if (canIUse('SystemCapability.AI.SpeechRecognizer')) {
    this.asrEngine = await speechRecognizer.createEngine({
      language: 'zh-CN',
      online: 1
    })
    // ... Engine initialization code ...
  }
}
```
```json
    "requestPermissions": [
      {
        "name": "ohos.permission.MICROPHONE",
        "reason": "$string:EntryAbility_label",
        "usedScene": {}
      }
    ],
```

### 4. Set Speech Recognition Callbacks
```typescript
this.asrEngine.setListener({
  onResult(sessionId: string, result: speechRecognizer.SpeechRecognitionResult) {
    _this.text = result.result  // Update the recognition result in real-time
    if (result.isLast) {        // Handle the end of recognition
      _this.isRecording = false
    }
  },
  // ... Other callback methods ...
})
```

### 5. Configure Audio Parameters
```typescript
const audioParam: speechRecognizer.AudioInfo = {
  audioType: 'pcm',      // Audio format
  sampleRate: 16000,     // Sampling rate
  soundChannel: 1,       // Mono channel
  sampleBit: 16          // Sampling bit depth
}
```

## Implementation Code:
### Complete Component Structure
```typescript
@Entry
@ComponentV2
struct SpeechRecognizer {
  @Local isRecording: boolean = false
  @Local text: string = ''
  hasPermissions: boolean = false
  asrEngine?: speechRecognizer.SpeechRecognitionEngine

  // Component lifecycle method
  aboutToAppear(): void {
    this.requestPermissions()
  }

  // ... Other method implementations ...
}
```

### UI Interaction Design
```typescript
build() {
  Column() {
    // Text display area
    Row() {
      Text(this.text)
        .width('100%')
    }

    // Long-press voice button
    Button(this.isRecording ? 'Start Speaking' : 'Press and Speak')
      .gesture(LongPressGesture()
        .onAction(() => this.startRecord())
        .onActionEnd(() => this.closeRecord()))
  }
}
```

## Summary:
### Key Points
- **Permission Management**: Dynamically request microphone permissions using `AbilityKit`.
- **Engine Lifecycle**: Initialize in the component's `aboutToAppear` method and release resources promptly after the operation.
- **Speech Recognition Process**:
  1. Create a recognition engine.
  2. Configure audio parameters (PCM format/16K sampling rate).
  3. Set result callbacks to update the UI in real-time.
  4. Use long-press gestures to control the start and stop of recognition.
- **Exception Handling**: Use `promptAction` to prompt permission exceptions and device busy status.

### Complete Code
```typescript
import { speechRecognizer } from '@kit.CoreSpeechKit'
import { abilityAccessCtrl } from '@kit.AbilityKit'
import { promptAction } from '@kit.ArkUI'


@Entry
@ComponentV2
struct SpeechRecognizer {
  @Local isRecording: boolean = false
  @Local text: string = ''
  hasPermissions: boolean = false
  asrEngine?: speechRecognizer.SpeechRecognitionEngine

  aboutToAppear(): void {
    // Request microphone permissions
    this.requestPermissions()
  }

  async requestPermissions() {
    const atManager = abilityAccessCtrl.createAtManager();
    const res = await atManager.requestPermissionsFromUser(getContext(), ['ohos.permission.MICROPHONE'])
    this.hasPermissions =
      res.authResults.every(grantStatus => grantStatus === abilityAccessCtrl.GrantStatus.PERMISSION_GRANTED)
  }

  // Start microphone recognition
  async startRecord() {
    if (canIUse('SystemCapability.AI.SpeechRecognizer')) {
      if (!this.hasPermissions) {
        return promptAction.showToast({ message: 'Microphone not authorized' })
      }
      if (this.isRecording) {
        return promptAction.showToast({ message: 'Recording...' })
      }
      this.isRecording = true
      this.asrEngine = await speechRecognizer.createEngine({
        language: 'zh-CN',
        online: 1
      })
      const _this = this
      this.asrEngine.setListener({
        onStart(sessionId: string, eventMessage: string) {
        },
        onEvent(sessionId: string, eventCode: number, eventMessage: string) {
        },
        onResult(sessionId: string, result: speechRecognizer.SpeechRecognitionResult) {
          _this.text = result.result
          if (result.isLast) {
            _this.isRecording = false
          }
        },
        onComplete(sessionId: string, eventMessage: string) {
        },
        onError(sessionId: string, errorCode: number, errorMessage: string) {
        }
      })
      const audioParam: speechRecognizer.AudioInfo = {
        audioType: 'pcm',
        sampleRate: 16000,
        soundChannel: 1,
        sampleBit: 16
      }
      const extraParam: Record<string, Object> = {
        "recognitionMode": 0,
        "vadBegin": 2000,
        "vadEnd": 3000,
        "maxAudioDuration": 20000
      }
      const recognizerParams: speechRecognizer.StartParams = {
        sessionId: '10000',
        audioInfo: audioParam,
        extraParams: extraParam
      }
      this.asrEngine.startListening(recognizerParams)
    }
  }

  async closeRecord() {
    if (canIUse('SystemCapability.AI.SpeechRecognizer')) {
      this.asrEngine?.finish('10000')
      this.asrEngine?.cancel('10000')
      this.asrEngine?.shutdown()
    }
  }

  build() {
    Column() {
      Row() {
        Text(this.text)
          .width('100%')
          .lineHeight(32)
      }
      .alignItems(VerticalAlign.Top)
      .width('100%')
      .layoutWeight(1)

      Button(this.isRecording ? 'Start Speaking' : 'Press and Speak')
        .width('100%')
        .gesture(LongPressGesture()
          .onAction(() => {
            this.startRecord()
          })
          .onActionEnd(() => {
            this.closeRecord()
          })
          .onActionCancel(() => {
            this.closeRecord()
          }))

    }
    .padding(15)
    .height('100%')
    .width('100%')
  }
}
```