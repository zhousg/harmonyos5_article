# HarmonyOS 5.0 中 VisionKit 图像分析器功能案例研究

## 内容摘要
本文介绍了如何在 HarmonyOS 5.0 中使用 `@kit.VisionKit` 的 `visionImageAnalyzer` 实现图像文字分析功能。通过创建 `VisionKitVisionImageAnalyzer` 组件，用户可以对指定图片进行文字分析，并展示全部文字和选择的文字内容。

## 实现步骤
1. 从 `@kit.VisionKit` 导入 `visionImageAnalyzer`。
2. 定义 `VisionKitVisionImageAnalyzer` 组件，初始化图像分析控制器和状态变量。
3. 在 `aboutToAppear` 生命周期函数中设置文字分析和选择文字变化的监听回调。
4. 构建 UI 界面，包含可分析的图片、展示全部文字的文本区域和展示选择文字的文本区域。

## 落地代码
```typescript
import { visionImageAnalyzer } from '@kit.VisionKit' 

@Entry 
@Component 
struct VisionKitVisionImageAnalyzer { 
  aiController = new visionImageAnalyzer.VisionImageAnalyzerController() 
  @State text: string = '' 
  @State selectedText: string = '' 

  aboutToAppear(): void { 
    this.aiController.on('textAnalysis', (text) => { 
      this.text = text 
    }) 
    this.aiController.on('selectedTextChange', (selectedText) => { 
      this.selectedText = selectedText 
    }) 
  } 

  build() { 
    Column({ space: 15 }) { 
      Image(' `https://inews.gtimg.com/om_ls/O3W2Lv10CTyLNHOjw4k_Co1Kkb2-c42GHWvifzD-ka5OYAA_294195/0` ', { 
        types: [ImageAnalyzerType.TEXT], 
        aiController: this.aiController 
      }) 
        .enableAnalyzer(true) 
        .objectFit(ImageFit.Contain) 
        .width(300) 
        .height(300) 
      Text('全部文字：' + this.text) 
      Text('选择文字：' + this.selectedText) 
    } 
    .alignItems(HorizontalAlign.Start) 
    .padding(15) 
    .height('100%') 
    .width('100%') 
  } 
} 
```

## 总结
本案例展示了在 HarmonyOS 5.0 中使用 `@kit.VisionKit` 的 `visionImageAnalyzer` 进行图像文字分析的方法。关键知识点包括图像分析控制器的初始化、监听回调的设置以及分析结果的展示。这些方法可用于开发需要图像文字分析功能的应用程序。