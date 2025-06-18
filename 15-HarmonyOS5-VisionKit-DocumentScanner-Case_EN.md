# Case Study of Document Scanner in VisionKit on HarmonyOS 5.0

## Abstract
This article introduces how to use `@hms.ai.DocumentScanner` to implement the document scanning function in HarmonyOS 5.0. By creating the `VisionKitDocumentScanner` component, users can perform document scanning and display the scanning results.

## Implementation Steps
1. Import relevant modules from `@kit.ArkUI` and `@hms.ai.DocumentScanner`.
2. Define the `VisionKitDocumentScanner` component and set the document image URI state.
3. Configure the document scanner parameters.
4. Build the UI interface, including the scanning result display area and the document scanner component.
5. Implement the callback function of the document scanner to handle the scanning results and update the document image URI state.

## Implementation Code
```typescript
import { router } from '@kit.ArkUI' 
import { DocumentScanner, DocumentScannerConfig } from '@hms.ai.DocumentScanner' 

@Entry 
@Component 
struct VisionKitDocumentScanner { 
  @State docImageUris: string[] = [] 
  private docScanConfig = new DocumentScannerConfig() 

  aboutToAppear(): void { 
    this.docScanConfig.maxShotCount = 3 
  } 

  build() { 
    Stack({ alignContent: Alignment.Top }) { 
      Swiper() { 
        ForEach(this.docImageUris, (uri: string) => { 
          Image(uri) 
            .objectFit(ImageFit.Contain) 
            .width(100) 
            .height(100) 
        }) 
      } 
      .padding(15) 
      .width('100%') 
      .height('100%') 

      // Document scanning 
      DocumentScanner({ 
        scannerConfig: this.docScanConfig, 
        onResult: (code, saveType, uris) => { 
          if (code === -1) { 
            router.back() 
          } 
          this.docImageUris = uris 
        } 
      }) 
    } 
    .height('100%') 
    .width('100%') 
  } 
} 
```

## Summary
This case demonstrates how to use `@hms.ai.DocumentScanner` to implement the document scanning function in HarmonyOS 5.0. The key knowledge points include the configuration and use of the document scanner, the handling of scanning results, and the display of scanning results. These methods can be used to develop various applications that require document scanning functions.