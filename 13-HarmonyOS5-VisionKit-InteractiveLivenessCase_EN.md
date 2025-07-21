# Case Study of VisionKit Interactive Liveness Detection in HarmonyOS 5.0

## Content Summary
This article introduces how to use `@kit.VisionKit` to implement the interactive liveness detection function in HarmonyOS 5.0. By creating the `VisionKitInteractiveLiveness` component, users can request camera permissions and start liveness detection.

## Implementation Steps
1. Import relevant modules from `@kit.AbilityKit`, `@kit.VisionKit`, and `@kit.ArkUI`.
2. Define the `VisionKitInteractiveLiveness` component and request camera permissions when the component is about to be displayed.
3. Implement the `requestPermissions` method to request camera permissions and check the authorization results.
4. Implement the `start` method to initiate interactive liveness detection after obtaining permissions.
5. Build the UI interface, which includes a "Start Detection" button.

## Implementation Code

- Configure permissions in `module.json5`

```json
"requestPermissions":[
  {
    "name": "ohos.permission.CAMERA",
    "reason": "$string:camera_desc",
    "usedScene": {"abilities": []}
  }
]
```

- Page code `VisionKitInteractiveLiveness.ets`

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
      // indicates that the detection mode is interactive, meaning the user needs to perform specified actions to verify if it's a real person or a spoof attack.
      // routeMode: 'back' as interactiveLiveness.RouteRedirectionMode, 
      // indicates the routing mode after detection is completed, which means returning to the previous page after detection.
      // actionsNum: 2, 
      // indicates the number of actions the user needs to perform to verify if it's a real person.
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
This case study demonstrates how to use `@kit.VisionKit` to implement interactive liveness detection in HarmonyOS 5.0. Key knowledge points include permission request and verification, configuration of liveness detection settings, and initiation of liveness detection. These methods can serve as a foundation for developing more complex biometric - related functions.

## Applicable Scenarios

The action - based liveness detection and card recognition capabilities are free during the trial period, which ends on December 31, 2026. Before the official charging begins, Huawei will announce the charging adjustment through official channels in advance.

Face liveness detection supports the action - based liveness detection mode. This mode can capture faces in real - time and requires the user to perform specified actions to verify if it's a real person or a spoof attack (e.g., printed photos, face - remade videos, and face masks).

Note: Liveness detection is a pure on - device algorithm and a free system - level service during the trial period. Developers are recommended to use it in low - risk business scenarios such as attendance checking, auxiliary login, and real - name authentication. The on - device algorithm has passed the certification of an authoritative organization (CFCA) in HarmonyOS NEXT/5.0.x. Given the high risks of payment and financial applications, developers are advised to conduct risk assessments and adopt necessary security measures based on the existing security features for different functional scenarios.

## Constraints and Limitations
- Supported text languages: Simplified Chinese, Traditional Chinese, English, Uyghur, and Tibetan.
- Supported voice - broadcast languages: Simplified Chinese and English.
- The face liveness detection service currently does not support landscape or split - screen detection.