## 案例描述
这是一个基于AI基础视觉服务实现的人脸对比案例，通过调用设备相册选择两张图片进行人脸特征比对，并展示相似度计算结果。

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/bcf64e7c0be64fa6aaa8af0efbe2e7ec.png)

## 实现步骤：
### 1. 模块导入
```typescript
// 导入功能模块
import { photoAccessHelper } from '@kit.MediaLibraryKit';
import { fileIo } from '@kit.CoreFileKit';
import { image } from '@kit.ImageKit';
import { faceComparator } from '@kit.CoreVisionKit';
import { promptAction } from '@kit.ArkUI';
import { BusinessError } from '@kit.BasicServicesKit';
```

### 2. 双图选择功能
```typescript
// 创建通用图片选择方法
async chooseImage (): Promise<PixelMap> {
  const photoPicker = new photoAccessHelper.PhotoViewPicker();
  const photoResult = await photoPicker.select({
    MIMEType: photoAccessHelper.PhotoViewMIMETypes.IMAGE_TYPE,
    maxSelectNumber: 1
  })
  // 获取选中图片文件句柄
  const fileSource = await fileIo.open(photoResult.photoUris[0], fileIo.OpenMode.READ_ONLY);
  // 生成像素图格式数据
  return await image.createImageSource(fileSource.fd).createPixelMap();
}
```

### 3. 图像处理流程
```typescript
// 双图存储变量定义
@Local chooseImage1?: PixelMap
@Local chooseImage2?: PixelMap

// 图片点击处理逻辑
.onClick(async () => {
  const chooseImage = await this.chooseImage()
  this.chooseImage1 = chooseImage // 第一张图存储
})

.onClick(async () => {
  const chooseImage = await this.chooseImage()
  this.chooseImage2 = chooseImage // 第二张图存储
})
```

### 4. 人脸对比核心实现
```typescript
// 配置双图对比参数
let visionInfo: faceComparator.VisionInfo = {
  pixelMap: this.chooseImage1,
};

let visionInfo1: faceComparator.VisionInfo = {
  pixelMap: this.chooseImage2,
};

// 执行人脸特征对比
faceComparator.compareFaces(visionInfo, visionInfo1)
  .then(result => {
    // 弹窗显示相似度结果
    promptAction.showDialog({ message: JSON.stringify(result) })
  })
  .catch((e: BusinessError) => {
    // 异常信息提示
    promptAction.showToast({ message: e.message })
  })
```

### 5. 检测结果展示
```typescript
// 弹窗显示结构化对比结果
promptAction.showDialog({
  message: JSON.stringify({
    similarity: 0.92, // 相似度值示例
    isSamePerson: true // 是否为同一人
  })
})
```

## 落地代码：
### 1. UI组件定义
```typescript
@Entry
@ComponentV2
struct FaceComparator {
  @Local chooseImage1?: PixelMap
  @Local chooseImage2?: PixelMap
```

### 2. 主功能方法
```typescript
// 整合双图选择、特征对比完整逻辑
async chooseImage(): Promise<PixelMap> {
  // 完整选择逻辑...
}
```

### 3. 界面构建
```typescript
build() {
  Column({ space: 20 }) {
    Image(this.chooseImage1)
      .onClick(/* 第一图选择 */)
    Image(this.chooseImage2)
      .onClick(/* 第二图选择 */)
    Button('人脸对比')
      .onClick(/* 触发对比逻辑 */)
  }
}
```

## 总结梳理：
### 核心点
1. 需要申请相册访问权限
2. 双图选择采用独立存储变量管理
3. compareFaces接口返回相似度(0-1)及特征点数据
4. 异常处理通过BusinessError捕获设备兼容性问题

### 完整代码
```typescript
import { photoAccessHelper } from '@kit.MediaLibraryKit';
import { fileIo } from '@kit.CoreFileKit';
import { image } from '@kit.ImageKit';
import { faceComparator } from '@kit.CoreVisionKit';
import { promptAction } from '@kit.ArkUI';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@ComponentV2
struct FaceComparator {
  @Local chooseImage1?: PixelMap
  @Local chooseImage2?: PixelMap

  async chooseImage (): Promise<PixelMap> {
    const photoPicker: photoAccessHelper.PhotoViewPicker = new photoAccessHelper.PhotoViewPicker();
    const photoResult = await photoPicker.select({
      MIMEType: photoAccessHelper.PhotoViewMIMETypes.IMAGE_TYPE,
      maxSelectNumber: 1
    })
    const photoUri = photoResult.photoUris[0]
    
    const fileSource = await fileIo.open(photoUri, fileIo.OpenMode.READ_ONLY);
    const imageSource = image.createImageSource(fileSource.fd);
    const chooseImage = await imageSource.createPixelMap();
    return chooseImage
  }

  build() {
    Column({ space: 20 }) {
      Image(this.chooseImage1)
        .alt($r('sys.media.save_button_picture'))
        .width(200)
        .aspectRatio(1)
        .onClick(async () => {
          const chooseImage = await this.chooseImage()
          this.chooseImage1 = chooseImage
        })
      Image(this.chooseImage2)
        .alt($r('sys.media.save_button_picture'))
        .width(200)
        .aspectRatio(1)
        .onClick(async () => {
          const chooseImage = await this.chooseImage()
          this.chooseImage2 = chooseImage
        })
      Button('人脸对比')
        .id('FaceComparatorButton')
        .onClick(async () => {
          if (this.chooseImage1 && this.chooseImage2) {
            let visionInfo: faceComparator.VisionInfo = {
              pixelMap: this.chooseImage1,
            };
            let visionInfo1: faceComparator.VisionInfo = {
              pixelMap: this.chooseImage2,
            };
            faceComparator.compareFaces(visionInfo, visionInfo1)
              .then(result => {
                promptAction.showDialog({ message: JSON.stringify(result) })
              })
              .catch((e: BusinessError) => {
                promptAction.showToast({ message: e.message })
              })
          }
        })
    }
    .padding(15)
    .height('100%')
    .width('100%')
  }
}
```