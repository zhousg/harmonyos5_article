# HarmonyOS 5.0 中 VisionKit 交互式活体检测案例

## 内容摘要
本文介绍了在 HarmonyOS 5.0 里如何使用 `@kit.VisionKit` 实现交互式活体检测功能。通过创建 `VisionKitInteractiveLiveness` 组件，用户可以请求相机权限并启动活体检测。

## 实现步骤
1. 导入 `@kit.AbilityKit`、`@kit.VisionKit` 和 `@kit.ArkUI` 中的相关模块。
2. 定义 `VisionKitInteractiveLiveness` 组件，在组件即将显示时请求相机权限。
3. 实现 `requestPermissions` 方法，用于请求相机权限并检查授权结果。
4. 实现 `start` 方法，在获得权限后启动交互式活体检测。
5. 构建 UI 界面，包含一个开始检测按钮。

## 落地代码

- 配置权限 `module.json5`

```json
"requestPermissions":[
  {
    "name": "ohos.permission.CAMERA",
    "reason": "$string:camera_desc",
    "usedScene": {"abilities": []}
  }
]
```

- 页面代码 `VisionKitInteractiveLiveness.ets`

```typescript
import { abilityAccessCtrl } from '@kit.AbilityKit'; 
import { interactiveLiveness } from '@kit.VisionKit'; 
import { promptAction, router } from '@kit.ArkUI'; 

@Entry 
@Component 
struct VisionKitInteractiveLiveness { 

  aboutToAppear(): void { 
    this.requestPermissions() 
  } 

  hasPermissions: boolean = false 

  async requestPermissions() { 
    const atManager = abilityAccessCtrl.createAtManager(); 
    const res = await atManager.requestPermissionsFromUser(getContext(), ['ohos.permission.CAMERA']) 
    this.hasPermissions = 
      res.authResults.every(grantStatus => grantStatus === abilityAccessCtrl.GrantStatus.PERMISSION_GRANTED) 
  } 

  async start () { 
    if (this.hasPermissions) { 
      const routerOptions: interactiveLiveness.
      InteractiveLivenessConfig = { 
        isSilentMode: 'INTERACTIVE_MODE' as interactiveLiveness.DetectionMode, 
        routeMode: 'back' as interactiveLiveness.RouteRedirectionMode, 
        actionsNum: 2, 
      } 
      // isSilentMode: 'INTERACTIVE_MODE' as interactiveLiveness.DetectionMode, 
      // 表示检测模式为交互式模式，即用户需要配合做指定动作才能判断是真实活体，还是非活体攻击。
      // routeMode: 'back' as interactiveLiveness.RouteRedirectionMode, 
      // 表示检测完成后返回的路由模式，即检测完成后返回上一个页面。
      // actionsNum: 2, 
      // 表示需要用户配合做的动作数量，即用户需要做两次指定动作才能判断是真实活体。
      await interactiveLiveness.startLivenessDetection(routerOptions) 
    } 
  } 

  build() { 
    Column() { 
      Button('开始检测') 
        .onClick(() => { 
          this.start() 
        }) 
    } 
    .height('100%') 
    .width('100%') 
  } 
} 
```

## 总结
本案例展示了在 HarmonyOS 5.0 中使用 `@kit.VisionKit` 实现交互式活体检测的方法。关键知识点包括权限请求与检查、活体检测配置的设置，以及活体检测的启动。这些方法可作为基础，用于开发更复杂的生物识别相关功能。

## 适用场景

人脸活体检测支持动作活体检测模式。动作活体检测支持实时捕捉人脸，需要用户配合做指定动作就可以判断是真实活体，还是非活体攻击（比如：打印图片、人脸翻拍视频以及人脸面具等）。

注意：活体检测是一项纯端侧算法、试用期免费的系统基础服务，推荐开发者使用在考勤打卡、辅助登录和实名认证等低危业务场景中。端侧算法在HarmonyOS NEXT/5.0.x已完成权威机构（CFCA）检测认证。鉴于支付和金融应用的高风险性，建议开发者基于现有的安全性，针对不同的功能场景进行风险评估和风控策略评估，并采取必要的安全措施。

## 约束限制
支持的文本语种类型：简体中文、繁体中文、英文、维吾尔文、藏文。
支持的播报语种类型：简体中文、英文。
人脸活体检测服务暂不支持横屏、分屏进行检测。