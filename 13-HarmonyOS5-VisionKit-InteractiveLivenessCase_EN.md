# Case Study of Interactive Liveness Detection in VisionKit on HarmonyOS 5.0

## Abstract
This article introduces how to use `@kit.VisionKit` to implement the interactive liveness detection function in HarmonyOS 5.0. By creating the `VisionKitInteractiveLiveness` component, users can request camera permissions and start liveness detection.

## Implementation Steps
1. Import relevant modules from `@kit.AbilityKit`, `@kit.VisionKit`, and `@kit.ArkUI`.
2. Define the `VisionKitInteractiveLiveness` component and request camera permissions when the component is about to appear.
3. Implement the `requestPermissions` method to request camera permissions and check the authorization results.
4. Implement the `start` method to start interactive liveness detection after obtaining permissions.
5. Build the UI interface, including a "Start Detection" button.

## Implementation Code
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
      Button('Start Detection') 
        .onClick(() => { 
          this.start() 
        }) 
    } 
    .height('100%') 
    .width('100%') 
  } 
} 
```

## Summary
This case demonstrates how to use `@kit.VisionKit` to implement interactive liveness detection in HarmonyOS 5.0. The key knowledge points include permission request and check, configuration of liveness detection settings, and starting the liveness detection process. These methods can serve as a foundation for developing more complex biometric - related functions.

## Application Scenarios
Face liveness detection supports the action-based liveness detection mode. This mode enables real-time face capture and requires users to perform specified actions to determine whether it is a real live person or a non-live attack (e.g., printed photos, face replay videos, and face masks).

Note: Liveness detection is a system-based service that uses on-device algorithms and is free during the trial period. Developers are recommended to use it in low-risk business scenarios such as attendance check-in, auxiliary login, and real-name authentication. The on-device algorithms have been tested and certified by an authoritative organization (CFCA) on HarmonyOS NEXT/5.0.x. Given the high risks associated with payment and financial applications, developers are advised to conduct risk assessments and risk control strategy evaluations based on the existing security measures for different functional scenarios and take necessary security measures.

## Constraints
Supported text languages: Simplified Chinese, Traditional Chinese, English, Uyghur, and Tibetan.
Supported broadcast languages: Simplified Chinese and English.
The face liveness detection service does not support detection in landscape mode or split-screen mode.