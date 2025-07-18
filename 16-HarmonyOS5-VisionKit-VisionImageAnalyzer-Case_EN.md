# Case Study of Vision Image Analyzer in VisionKit on HarmonyOS 5.0

## Abstract
This article introduces how to use `visionImageAnalyzer` from `@kit.VisionKit` to implement image text analysis in HarmonyOS 5.0. By creating the `VisionKitVisionImageAnalyzer` component, users can perform text analysis on specified images and display all the text and the selected text.

## Implementation Steps
1. Import `visionImageAnalyzer` from `@kit.VisionKit`.
2. Define the `VisionKitVisionImageAnalyzer` component and initialize the image analysis controller and state variables.
3. Set up listeners for text analysis and selected text change in the `aboutToAppear` lifecycle function.
4. Build the UI interface, including an analyzable image, a text area to display all text, and a text area to display selected text.

## Implementation Code
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
      Text('All text: ' + this.text) 
      Text('Selected text: ' + this.selectedText) 
    } 
    .alignItems(HorizontalAlign.Start) 
    .padding(15) 
    .height('100%') 
    .width('100%') 
  } 
} 
```

## Summary
This case demonstrates how to use `visionImageAnalyzer` from `@kit.VisionKit` for image text analysis in HarmonyOS 5.0. The key knowledge points include the initialization of the image analysis controller, the setup of listener callbacks, and the display of analysis results. These methods can be used to develop applications that require image text analysis functions.

## Application Scenarios
AI image recognition aggregates AI capabilities such as OCR (Optical Character Recognition), subject segmentation, entity recognition, and multi - object recognition to provide scene - based text recognition, subject segmentation, and image recognition search functions. The main switch for the AI image recognition function is located in the list of basic control APIs. If you accept the default interaction and functions of AI image recognition, you only need to use the relevant enabling interfaces provided by the basic controls to turn on the function switch. The APIs supporting this document are used in conjunction with basic controls, mainly to meet your customization requirements, help you achieve fine - grained control over the interaction of the AI image recognition function, and obtain analysis results such as text recognition and image segmentation for the development of extended services. Currently, the supported basic controls include Image, Video, and XComponent. Specifically, in combination with the Image control, the image recognition function for static images can be achieved; in combination with the Video control, the image recognition function for paused frames during video playback can be achieved; in combination with the XComponent, the image recognition function in scenarios such as custom rendering can be achieved.

## Constraints
Supported text languages: Simplified Chinese, Traditional Chinese, English, Uyghur, and Tibetan.
The minimum supported image resolution is 100*100.
The images to be analyzed must be static non - vector images. That is, image types such as SVG and GIF are not supported for analysis. PixelMap can be passed for analysis, and currently only the RGBA_8888 type is supported.
The minimum width - to - height ratio for translatable images is 1:3 (the height is less than 3 times the width), and the minimum width - to - height ratio for text - recognizable images is 1:7 (the height is less than 7 times the width).