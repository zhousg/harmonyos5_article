# Case Study of VisionKit Document Scanning Function in HarmonyOS 5.0

## Abstract
This article introduces how to use `@hms.ai.DocumentScanner` to implement the document scanning function in HarmonyOS 5.0. By creating the `VisionKitDocumentScanner` component, users can perform document scanning and display the scanning results.

## Implementation Steps
1. Import relevant modules from `@kit.ArkUI` and `@hms.ai.DocumentScanner`.
```typescript
import { DocumentScanner, ScanConfig } from "@hms.ai.DocumentScanner";
import { router } from "@kit.ArkUI";
```
2. Define the `VisionKitDocumentScanner` component and set the document image URI state.
```typescript
@Entry
@Component
struct VisionKitDocumentScanner {
  @State docImageUri: string = '';
}
```
3. Configure the document scanner parameters.
```typescript
const scanConfig: ScanConfig = {
  scanMode: 0, // 0: Auto mode, 1: Manual mode
  outputFormat: 0, // 0: JPEG, 1: PNG
  quality: 90
};
```
4. Build the UI interface, including the scanning result display area and the document scanner component.
```typescript
  build() {
    NavDestination() {
      Stack({ alignContent: Alignment.Top }) {
        if (this.docImageUri) {
          Image(this.docImageUri)
            .objectFit(ImageFit.Contain)
            .width('80%')
            .height('80%')
        }

        DocumentScanner({
          scanConfig: scanConfig,
          onScanResult: this.onScanResult
        })
        .width('100%')
        .height('100%')
      }
      .width('100%')
      .height('100%')
    }
    .width('100%')
    .height('100%')
    .hideTitleBar(true)
  }
```
5. Implement the callback function of the document scanner to handle the scanning results and update the document image URI state.
```typescript
  onScanResult = (result) => {
    if (result.code === 0) {
      this.docImageUri = result.imageUri;
    } else {
      console.error('Scan failed:', result.message);
      router.back();
    }
  }
```

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

## Other Configurations

```typescript
aboutToAppear() {
    this.docScanConfig.supportType = [DocType.DOC, DocType.SHEET]
    this.docScanConfig.isGallerySupported = true
    this.docScanConfig.editTabs = []
    this.docScanConfig.maxShotCount = 3
    this.docScanConfig.defaultFilterId = FilterId.ORIGINAL
    this.docScanConfig.defaultShootingMode = ShootingMode.MANUAL
    this.docScanConfig.isShareable = true
    this.docScanConfig.originalUris = []
  }
  // Explanation:
  // supportType: Supported document types, including DocType.DOC and DocType.SHEET
  // isGallerySupported: Whether to support selecting images from the gallery
  // editTabs: Edit tabs, supporting DocEditTab.CROP, DocEditTab.ROTATE, and DocEditTab.FILTER
  // maxShotCount: Maximum number of shots
  // defaultFilterId: Default filter ID
  // defaultShootingMode: Default shooting mode
  // isShareable: Whether to support sharing
  // originalUris: List of original image URIs
```

## Summary
This case demonstrates how to use `@hms.ai.DocumentScanner` to implement the document scanning function in HarmonyOS 5.0. Key knowledge points include the configuration and use of the document scanner, the handling of scanning results, and the display of scanning results. These methods can be used to develop various applications that require document scanning functions.

# Application Scenarios
The document scanning control provides a service to capture documents and convert them into high - definition scanned copies. Users only need to use their mobile phones to take pictures of documents, which will be automatically cropped and optimized. It supports saving and sharing in image and PDF formats. Meanwhile, it supports capturing images or selecting images from the gallery to recognize tables and generate table documents.

It can be widely used in educational and office scenarios. Users can scan documents, bills, classroom PPTs, books, etc., and output images/PDFs to complete operations such as sending and archiving.

## Constraints
Supported languages: Simplified Chinese, English.
Currently, document scanning only supports phones and tablets.
It is not allowed to be blocked by other components or windows.