
## 案例描述
这是一个基于AI基础语音服务实现的实时语音转文字案例，通过麦克风采集音频并实时转换为文本。

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/005f6cb30e1f43f8aa623e7f08c92bf3.png)


## 实现步骤：
### 1. 导入必要模块
```typescript
import { speechRecognizer } from '@kit.CoreSpeechKit'
import { abilityAccessCtrl } from '@kit.AbilityKit'
import { promptAction } from '@kit.ArkUI'
```

### 2. 申请麦克风权限
```typescript
async requestPermissions() {
  const atManager = abilityAccessCtrl.createAtManager();
  const res = await atManager.requestPermissionsFromUser(getContext(), ['ohos.permission.MICROPHONE'])
  this.hasPermissions = 
    res.authResults.every(grantStatus => grantStatus === abilityAccessCtrl.GrantStatus.PERMISSION_GRANTED)
}
```

### 3. 初始化语音识别引擎
```typescript
async startRecord() {
  if (canIUse('SystemCapability.AI.SpeechRecognizer')) {
    this.asrEngine = await speechRecognizer.createEngine({
      language: 'zh-CN',
      online: 1
    })
    // ...引擎初始化代码...
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

### 4. 设置语音识别回调
```typescript
this.asrEngine.setListener({
  onResult(sessionId: string, result: speechRecognizer.SpeechRecognitionResult) {
    _this.text = result.result  // 实时更新识别结果
    if (result.isLast) {        // 识别结束处理
      _this.isRecording = false
    }
  },
  // ...其他回调方法...
})
```

### 5. 配置音频参数
```typescript
const audioParam: speechRecognizer.AudioInfo = {
  audioType: 'pcm',      // 音频格式
  sampleRate: 16000,     // 采样率
  soundChannel: 1,       // 单声道
  sampleBit: 16          // 采样位数
}
```

## 落地代码：
### 完整组件结构
```typescript
@Entry
@ComponentV2
struct SpeechRecognizer {
  @Local isRecording: boolean = false
  @Local text: string = ''
  hasPermissions: boolean = false
  asrEngine?: speechRecognizer.SpeechRecognitionEngine

  // 组件生命周期方法
  aboutToAppear(): void {
    this.requestPermissions()
  }

  // ...其他方法实现...
}
```

### UI交互设计
```typescript
build() {
  Column() {
    // 文本展示区域
    Row() {
      Text(this.text)
        .width('100%')
    }

    // 长按语音按钮
    Button(this.isRecording ? '开始 说话' : '按住 说话')
      .gesture(LongPressGesture()
        .onAction(() => this.startRecord())
        .onActionEnd(() => this.closeRecord()))
  }
}
```

## 总结梳理：
### 核心点
- **权限管理**：使用`AbilityKit`动态申请麦克风权限
- **引擎生命周期**：在组件`aboutToAppear`初始化，操作结束及时释放资源
- **语音识别流程**：
  1. 创建识别引擎
  2. 配置音频参数（PCM格式/16K采样率）
  3. 设置结果回调实时更新UI
  4. 长按手势控制识别启停
- **异常处理**：通过`promptAction`提示权限异常和设备忙状态

### 完整代码
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
    // 获取麦克风权限
    this.requestPermissions()
  }

  async requestPermissions() {
    const atManager = abilityAccessCtrl.createAtManager();
    const res = await atManager.requestPermissionsFromUser(getContext(), ['ohos.permission.MICROPHONE'])
    this.hasPermissions =
      res.authResults.every(grantStatus => grantStatus === abilityAccessCtrl.GrantStatus.PERMISSION_GRANTED)
  }

  // 开始麦克风识别
  async startRecord() {
    if (canIUse('SystemCapability.AI.SpeechRecognizer')) {
      if (!this.hasPermissions) {
        return promptAction.showToast({ message: '麦克风未授权' })
      }
      if (this.isRecording) {
        return promptAction.showToast({ message: '正在录制...' })
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

      Button(this.isRecording ? '开始 说话' : '按住 说话')
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