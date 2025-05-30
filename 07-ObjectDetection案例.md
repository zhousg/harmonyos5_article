## 内容摘要
本文介绍了一个基于HarmonyOS 5.0和ArkTS语言开发的目标检测应用。该应用允许用户选择图片，利用系统的目标检测能力识别图片中的物体，并在图片上绘制识别区域，同时展示识别结果的可信度、标签和尺寸信息。

## 实现步骤
1. 导入必要的模块。
2. 定义标签映射表。
3. 创建目标检测组件。
4. 初始化组件状态。
5. 实现图片选择功能。
6. 进行目标识别。
7. 绘制识别区域。
8. 展示识别结果。

## 落地代码
### 1. 模块导入
```typescript:/Users/zhousg/Documents/文章/07-ObjectDetection案例.md
import { photoAccessHelper } from '@kit.MediaLibraryKit'
import { fileIo } from '@kit.CoreFileKit'
import image from '@ohos.multimedia.image'
import { objectDetection, visionBase } from '@kit.CoreVisionKit'
import { promptAction } from '@kit.ArkUI';
```

### 2. 定义标签映射表
```typescript:/Users/zhousg/Documents/文章/07-ObjectDetection案例.md
const LabelMap: Record<number, string> = {
  0: "风景",
  1: "动物",
  2: "植物",
  3: "建筑",
  5: "人脸",
  6: "表格",
  7: "文本",
  8: "人头",
  9: "猫头",
  10: "狗头",
  11: "食物",
  12: "汽车",
  13: "人体",
  21: "文档",
  22: "卡证"
};
```

### 3. 创建目标检测组件
```typescript:/Users/zhousg/Documents/文章/07-ObjectDetection案例.md
@Entry
@ComponentV2
struct ObjectDetection {
  @Local chooseImage?: PixelMap
  @Local objects: objectDetection.VisionObject[] = []
  @Local canvasWidth: number = 320
  @Local canvasHeight: number = 200
  @Local bozaiImage?: PixelMap
```

### 4. 初始化组件状态
```typescript:/Users/zhousg/Documents/文章/07-ObjectDetection案例.md
  async aboutToAppear(): Promise<void> {
    const resManager = getContext()
      .resourceManager
    const bozaiArray = await resManager.getMediaContent($r('app.media.bozai'))
    const bozaiResource = image.createImageSource(bozaiArray.buffer)
    this.bozaiImage = await bozaiResource.createPixelMap()
  }
```

### 5. 实现图片选择功能
```typescript:/Users/zhousg/Documents/文章/07-ObjectDetection案例.md
  async checkImage() {
    // 1. 选择图片
    const photoPicker = new photoAccessHelper.PhotoViewPicker()
    const result = await photoPicker.select({ 
      MIMEType: photoAccessHelper.PhotoViewMIMETypes.IMAGE_TYPE,
      maxSelectNumber: 1
    })
    const uri = result.photoUris[0]
    if (uri) {
      const imageFile = await fileIo.open(uri, fileIo.OpenMode.READ_ONLY)
      const imageSource = image.createImageSource(imageFile.fd)
      this.chooseImage = await imageSource.createPixelMap()
```

### 6. 进行目标识别
```typescript:/Users/zhousg/Documents/文章/07-ObjectDetection案例.md
      // 2. 进行识别
      const request: visionBase.Request = {
        inputData: { pixelMap: this.chooseImage }
      }
      if (canIUse('SystemCapability.AI.Vision.ObjectDetection')) {
        const objectDetector = await objectDetection.ObjectDetector.create()
        const data = await objectDetector.process(request)
        this.objects = data.objects
```

### 7. 绘制识别区域
```typescript:/Users/zhousg/Documents/文章/07-ObjectDetection案例.md
        // 3. 绘制识别区域
        const imageInfo = await this.chooseImage.getImageInfo()
        const ratio = 320 / imageInfo.size.width
        this.canvasHeight = imageInfo.size.height * ratio
        this.canvasContext.drawImage(this.chooseImage, 0, 0, this.canvasWidth, this.canvasHeight)
        this.objects.forEach(item => {
          if (item.labels.includes(5)) {
            const box = item.boundingBox
            this.canvasContext.drawImage(this.bozaiImage, box.left * ratio - ( box.height * ratio * 0.15 ), box.top * ratio, box.height * ratio,
              box.height * ratio)
          }
        })
```

### 8. 展示识别结果
```typescript:/Users/zhousg/Documents/文章/07-ObjectDetection案例.md
  build() {
    Column({ space: 20 }) {
      Canvas(this.canvasContext) {

      }
      .width(this.canvasWidth)
      .height(this.canvasHeight)
      .backgroundColor('#CCCCCC')

      Button('选择图片，进行识别')
        .onClick(() => {
          this.checkImage()
        })
      List({ space: 20 }) {
        ForEach(this.objects, (item: objectDetection.VisionObject) => {
          ListItem() {
            Column() {
              Text('可信度：' + item.score)
                .width('100%')
              Text('标签：' + item.labels.map(label => LabelMap[label])
                .join('，'))
                .width('100%')
              Text('尺寸：' + JSON.stringify(item.boundingBox))
                .width('100%')
            }
          }
        })
      }
      .width('100%')
      .height('100%')
      .layoutWeight(1)
    }
    .padding(15)
    .height('100%')
    .width('100%')
  }
}
```

## 总结
- 利用 `@kit` 系列工具包实现了图片选择、文件操作、图像处理和目标检测功能。
- 通过 `ObjectDetector` 类进行目标识别，获取识别结果。
- 使用 `Canvas` 组件绘制图片和识别区域，直观展示识别结果。
- 利用 `List` 组件展示识别结果的详细信息，包括可信度、标签和尺寸。