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

## 适用场景
AI识图是通过聚合OCR（Optical Character Recognition）、主体分割、实体识别、多目标识别等AI能力，提供场景化的文本识别、主体分割、识图搜索功能。AI识图功能主开关入口在基础控件API列表中，如果您接受AI识图默认的交互和功能，仅需使用基础控件提供的相关使能接口打开功能开关即可。该文档配套的API配合基础控件使用，主要满足您的定制诉求，帮助您完成AI识图功能交互上的细粒度控制，获取文本识别、图像分割等分析结果以便您进行扩展业务的开发，目前支持的基础控件范围包括Image、Video、XComponent。其中，配合Image控件可完成静态图片上的识图功能，配合Video控件可完成视频播放暂停帧的识图功能，配合XComponent可完成自定义渲染等场景下的图像的识图功能。

## 约束限制

支持的文本语种类型：简体中文、繁体中文、英文、维吾尔文、藏文。
支持图片最小规格100*100分辨率。
分析图像要求是静态非矢量图，即svg、gif等图像类型不支持分析，支持传入PixelMap进行分析，目前仅支持RGBA_8888类型。
支持翻译的图片宽高最小比例为1:3（高度小于宽度的3倍），支持文本识别的图片宽高最小比例为1:7（高度小于宽度的7倍）。