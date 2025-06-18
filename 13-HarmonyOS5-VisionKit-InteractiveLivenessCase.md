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
      const routerOptions: interactiveLiveness.InteractiveLivenessConfig = { 
        isSilentMode: 'INTERACTIVE_MODE' as interactiveLiveness.DetectionMode, 
        routeMode: 'back' as interactiveLiveness.RouteRedirectionMode, 
        actionsNum: 2, 
      } 
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