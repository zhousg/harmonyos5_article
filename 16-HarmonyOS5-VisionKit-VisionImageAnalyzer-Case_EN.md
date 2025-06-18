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