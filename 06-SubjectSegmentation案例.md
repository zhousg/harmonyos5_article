## 案例描述
这是一个基于AI基础视觉服务实现的背景替换案例，通过调用设备相册选择图片后对主体进行智能分割，并支持动态更换背景颜色。

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/03f6abcd8e93452f8611b79f4664aa63.png)

## 实现步骤：
### 1. 模块导入与组件定义
```typescript
import { photoAccessHelper } from '@kit.MediaLibraryKit'
import { fileIo } from '@kit.CoreFileKit'
import image from '@ohos.multimedia.image'
import { subjectSegmentation } from '@kit.CoreVisionKit'
import { promptAction } from '@kit.ArkUI'

@Entry
@ComponentV2
struct SubjectSegmentation {
  @Local chooseImage?: PixelMap  // 原始图片
  @Local segmentedImage?: PixelMap // 分割后图片
  @Local bgColor: ResourceColor = Color.White // 背景色状态
```

### 2. 图片选择与处理
```typescript
async segmentImage() {
  if (canIUse('SystemCapability.AI.Vision.SubjectSegmentation')) {
    // 创建图片选择器
    const photoPicker: photoAccessHelper.PhotoViewPicker = new photoAccessHelper.PhotoViewPicker();
    
    // 选择单张图片
    const photoResult = await photoPicker.select({
      MIMEType: photoAccessHelper.PhotoViewMIMETypes.IMAGE_TYPE,
      maxSelectNumber: 1
    })
    
    // 获取图片URI并转换为PixelMap格式
    const photoUri = photoResult.photoUris[0]
    const fileSource = await fileIo.open(photoUri, fileIo.OpenMode.READ_ONLY);
    const imageSource = image.createImageSource(fileSource.fd);
    this.chooseImage = await imageSource.createPixelMap();
```

### 3. 主题分割处理
```typescript
    // 配置视觉识别参数
    const visionInfo: subjectSegmentation.VisionInfo = {
      pixelMap: this.chooseImage,
    };
    
    try {
      // 执行主体分割
      const result = await subjectSegmentation.doSegmentation(visionInfo, {
        enableSubjectForegroundImage: true
      })
      this.segmentedImage = result.fullSubject.foregroundImage
    } catch (e) {
      promptAction.showToast({ message: e.message })
    }
  }
}
```

### 4. 背景替换机制
```typescript
  build() {
    Column({ space: 20 }) {
      // 原始图片展示区域
      Text('原图：')
      Image(this.chooseImage)
        .objectFit(ImageFit.Fill)
        .height('30%')
      
      // 分割后图片展示区域
      Text('扣除背景：')
      Image(this.segmentedImage)
        .objectFit(ImageFit.Fill)
        .height('30%')
        .backgroundColor(this.bgColor)
      
      // 功能操作按钮
      Button('选择图片').onClick(() => this.segmentImage())
      Button('更换背景').onClick(() => {
        // 生成随机RGB背景色
        const a = Math.round(Math.random() * 255)
        const b = Math.round(Math.random() * 255)
        const c = Math.round(Math.random() * 255)
        this.bgColor = `rgb(${a},${b},${c})`
      })
    }
    .padding(15)
    .height('100%')
    .width('100%')
  }
}
```

## 总结梳理：
### 核心点
1. 多媒体库调用实现图片选择与格式转换
2. subjectSegmentation.doSegmentation接口完成智能主体分割
3. 动态背景色机制通过随机RGB值实现

### 完整代码
```typescript
// 此处完整保留用户提供的原始代码
import { photoAccessHelper } from '@kit.MediaLibraryKit'
import { fileIo } from '@kit.CoreFileKit'
import image from '@ohos.multimedia.image'
import { subjectSegmentation } from '@kit.CoreVisionKit'
import { promptAction } from '@kit.ArkUI'

@Entry
@ComponentV2
struct SubjectSegmentation {
  @Local chooseImage?: PixelMap
  @Local segmentedImage?: PixelMap
  @Local bgColor: ResourceColor = Color.White

  async segmentImage() {
    if (canIUse('SystemCapability.AI.Vision.SubjectSegmentation')) {
      const photoPicker: photoAccessHelper.PhotoViewPicker = new photoAccessHelper.PhotoViewPicker();
      const photoResult = await photoPicker.select({
        MIMEType: photoAccessHelper.PhotoViewMIMETypes.IMAGE_TYPE,
        maxSelectNumber: 1
      })
      const photoUri = photoResult.photoUris[0]
      const fileSource = await fileIo.open(photoUri, fileIo.OpenMode.READ_ONLY);
      const imageSource = image.createImageSource(fileSource.fd);
      this.chooseImage = await imageSource.createPixelMap();
      const visionInfo: subjectSegmentation.VisionInfo = {
        pixelMap: this.chooseImage,
      };
      try {
        const result = await subjectSegmentation.doSegmentation(visionInfo, {
          enableSubjectForegroundImage: true
        })
        this.segmentedImage = result.fullSubject.foregroundImage
      } catch (e) {
        promptAction.showToast({ message: e.message })
      }
    }
  }

  build() {
    Column({ space: 20 }) {
      Text('原图：')
      Image(this.chooseImage)
        .objectFit(ImageFit.Fill)
        .height('30%')
      Text('扣除背景：')
      Image(this.segmentedImage)
        .objectFit(ImageFit.Fill)
        .height('30%')
        .backgroundColor(this.bgColor)
      Button('选择图片')
        .onClick(() => this.segmentImage())
      Button('更换背景')
        .onClick(() => {
          const a = Math.round(Math.random() * 255)
          const b = Math.round(Math.random() * 255)
          const c = Math.round(Math.random() * 255)
          this.bgColor = `rgb(${a},${b},${c})`
        })
    }
    .padding(15)
    .height('100%')
    .width('100%')
  }
}
```