## Content Summary
This article introduces an object detection application developed based on HarmonyOS 5.0 and the ArkTS language. This application allows users to select images, utilize the system's object detection capabilities to identify objects in the images, draw detection areas on the images, and display detailed information about the detection results, including confidence levels, labels, and dimensions.

## Implementation Steps
1. Import necessary modules.
2. Define a label mapping table.
3. Create an object detection component.
4. Initialize the component state.
5. Implement the image selection function.
6. Perform object recognition.
7. Draw the detection areas.
8. Display the detection results.

## Implementation Code
### 1. Module Import
```typescript:/Users/zhousg/Documents/文章/07-ObjectDetection案例_en.md
import { photoAccessHelper } from '@kit.MediaLibraryKit'
import { fileIo } from '@kit.CoreFileKit'
import image from '@ohos.multimedia.image'
import { objectDetection, visionBase } from '@kit.CoreVisionKit'
import { promptAction } from '@kit.ArkUI';
```

### 2. Define a Label Mapping Table
```typescript:/Users/zhousg/Documents/文章/07-ObjectDetection案例_en.md
const LabelMap: Record<number, string> = {
  0: "Scenery",
  1: "Animal",
  2: "Plant",
  3: "Building",
  5: "Face",
  6: "Table",
  7: "Text",
  8: "Human Head",
  9: "Cat Head",
  10: "Dog Head",
  11: "Food",
  12: "Car",
  13: "Human Body",
  21: "Document",
  22: "Card"
};
```

### 3. Create an Object Detection Component
```typescript:/Users/zhousg/Documents/文章/07-ObjectDetection案例_en.md
@Entry
@ComponentV2
struct ObjectDetection {
  @Local chooseImage?: PixelMap
  @Local objects: objectDetection.VisionObject[] = []
  @Local canvasWidth: number = 320
  @Local canvasHeight: number = 200
  @Local bozaiImage?: PixelMap
```

### 4. Initialize the Component State
```typescript:/Users/zhousg/Documents/文章/07-ObjectDetection案例_en.md
  async aboutToAppear(): Promise<void> {
    const resManager = getContext()
      .resourceManager
    const bozaiArray = await resManager.getMediaContent($r('app.media.bozai'))
    const bozaiResource = image.createImageSource(bozaiArray.buffer)
    this.bozaiImage = await bozaiResource.createPixelMap()
  }
```

### 5. Implement the Image Selection Function
```typescript:/Users/zhousg/Documents/文章/07-ObjectDetection案例_en.md
  async checkImage() {
    // 1. Select an image
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

### 6. Perform Object Recognition
```typescript:/Users/zhousg/Documents/文章/07-ObjectDetection案例_en.md
      // 2. Perform recognition
      const request: visionBase.Request = {
        inputData: { pixelMap: this.chooseImage }
      }
      if (canIUse('SystemCapability.AI.Vision.ObjectDetection')) {
        const objectDetector = await objectDetection.ObjectDetector.create()
        const data = await objectDetector.process(request)
        this.objects = data.objects
```

### 7. Draw the Detection Areas
```typescript:/Users/zhousg/Documents/文章/07-ObjectDetection案例_en.md
        // 3. Draw the detection areas
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

### 8. Display the Detection Results
```typescript:/Users/zhousg/Documents/文章/07-ObjectDetection案例_en.md
  build() {
    Column({ space: 20 }) {
      Canvas(this.canvasContext) {

      }
      .width(this.canvasWidth)
      .height(this.canvasHeight)
      .backgroundColor('#CCCCCC')

      Button('Select an image for recognition')
        .onClick(() => {
          this.checkImage()
        })
      List({ space: 20 }) {
        ForEach(this.objects, (item: objectDetection.VisionObject) => {
          ListItem() {
            Column() {
              Text('Confidence: ' + item.score)
                .width('100%')
              Text('Labels: ' + item.labels.map(label => LabelMap[label])
                .join(', '))
                .width('100%')
              Text('Dimensions: ' + JSON.stringify(item.boundingBox))
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

## Summary
- Utilized the `@kit` series of toolkits to implement image selection, file operations, image processing, and object detection functions.
- Performed object recognition using the `ObjectDetector` class to obtain detection results.
- Used the `Canvas` component to draw images and detection areas, visually displaying the detection results.
- Utilized the `List` component to display detailed information about the detection results, including confidence levels, labels, and dimensions.