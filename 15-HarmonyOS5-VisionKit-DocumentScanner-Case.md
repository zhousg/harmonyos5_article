# HarmonyOS 5.0 中 VisionKit 文档扫描功能案例研究

## 内容摘要
本文介绍了如何在 HarmonyOS 5.0 中使用 `@hms.ai.DocumentScanner` 实现文档扫描功能。通过创建 `VisionKitDocumentScanner` 组件，用户可以进行文档扫描并展示扫描结果。

## 实现步骤
1. 从 `@kit.ArkUI` 和 `@hms.ai.DocumentScanner` 导入相关模块。
```typescript
import { DocumentScanner, ScanConfig } from "@hms.ai.DocumentScanner";
import { router } from "@kit.ArkUI";
```
2. 定义 `VisionKitDocumentScanner` 组件并设置文档图片 URI 状态。
```typescript
@Entry
@Component
struct VisionKitDocumentScanner {
  @State docImageUri: string = '';
}
```
3. 配置文档扫描器参数。
```typescript
const scanConfig: ScanConfig = {
  scanMode: 0, // 0: Auto mode, 1: Manual mode
  outputFormat: 0, // 0: JPEG, 1: PNG
  quality: 90
};
```
4. 构建 UI 界面，包含扫描结果展示区域和文档扫描器组件。
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
5. 实现文档扫描器的回调函数，处理扫描结果并更新文档图片 URI 状态。
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

## 落地代码
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

      // 文档扫描 
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

## 其他配置

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
  // 解释：
  // supportType：支持的文档类型，支持 DocType.DOC 和 DocType.SHEET
  // isGallerySupported：是否支持从图库选择图片
  // editTabs：编辑选项卡，支持 DocEditTab.CROP、DocEditTab.ROTATE、DocEditTab.FILTER
  // maxShotCount：最大拍摄次数
  // defaultFilterId：默认的滤镜 ID
  // defaultShootingMode：默认的拍摄模式
  // isShareable：是否支持分享
  // originalUris：原始图片 URI 列表
```

## 总结
本案例展示了如何在 HarmonyOS 5.0 中使用 `@hms.ai.DocumentScanner` 实现文档扫描功能。关键知识点包括文档扫描器的配置和使用、扫描结果的处理以及扫描结果的展示。这些方法可用于开发各种需要文档扫描功能的应用程序。


# 适用场景
文档扫描控件提供拍摄文档并转换为高清扫描件的服务。仅需使用手机拍摄文档，即可自动裁剪和优化，并支持图片、PDF格式保存和分享；同时支持拍摄或从图库选择图片识别表格，生成表格文档。

可广泛用于教育办公场景，扫描文档、票据、课堂PPT和书籍等输出图片/PDF供用户完成发送、存档等操作。

## 约束限制

支持的语种类型：简体中文、英文。
文档扫描暂时只支持phone、tablet设备。
不允许被其他组件或窗口遮挡。