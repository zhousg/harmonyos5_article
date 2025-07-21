# Skeleton Detection Case Article

## Summary
This article primarily introduces a case of implementing skeleton detection using the ArkTS language on HarmonyOS 5.0. By leveraging `visionBase` and `skeletonDetector`, the detection of skeletal key points is carried out. Meanwhile, the confidence threshold is lowered to enhance the flexibility of detection.

## Application Scenarios
Core Vision Kit (Basic Vision Service) provides basic capabilities related to machine vision, such as general text recognition (i.e., OCR, Optical Character Recognition), face detection, face comparison, and subject segmentation.

Human skeleton key point detection mainly detects some key points of the human body and describes human skeletal information through these points. Specific applications are mainly concentrated in intelligent video surveillance, patient monitoring systems, human-computer interaction, virtual reality, human animation, smart homes, intelligent security, athlete-assisted training, etc.

It supports the recognition of 17 key points, specifically the nose, left and right eyes, left and right ears, left and right shoulders, left and right elbows, left and right wrists, left and right hips, left and right knees, and left and right ankles.

## Implementation Steps
1. Define the request object.
2. Call the skeleton detection processing method.
3. Extract key point data.
4. Obtain specific key points based on their types.

## Implementation Code
### 1. Define the Request Object
```typescript
const request: visionBase.Request = {
  inputData: { pixelMap }
}
```

### 2. Call the Skeleton Detection Processing Method
```typescript
const data = await skeletonDetector.process(request)
```

### 3. Extract Key Point Data
```typescript
const points = data.skeletons[0].points
```

### 4. Lower the Confidence Threshold and Define the Method to Obtain Key Points
```typescript
const MIN_CONFIDENCE = 0.3; // Lower the confidence threshold
const getPoint = (type: skeletonDetection.SkeletonPointType) => points.find(p => p.type === type && p.score >= MIN_CONFIDENCE)?.point;
```

### 5. Obtain Specific Key Points
```typescript
// Obtain key points (prefer the nose, then the ears)
const nose = getPoint(skeletonDetection.SkeletonPointType.NOSE);
const leftShoulder = getPoint(skeletonDetection.SkeletonPointType.LEFT_SHOULDER);
const rightShoulder = getPoint(skeletonDetection.SkeletonPointType.RIGHT_SHOULDER);
let leftEar = getPoint(skeletonDetection.SkeletonPointType.LEFT_EAR);
let rightEar = getPoint(skeletonDetection.SkeletonPointType.RIGHT_EAR);
```

## Constraints
The input image should have appropriate imaging quality (720p or higher is recommended). The height should be between 100px and 10000px, and the width should be between 100px and 10000px. The aspect ratio is recommended to be below 5:1 (the height is less than 5 times the width), and it is advisable to be close to the aspect ratio of a mobile phone screen.

## Conclusion
The key knowledge point of this case lies in how to perform skeleton detection using the ArkTS language on HarmonyOS 5.0. By defining the request object, calling the processing method, and extracting key point data, the basic skeleton detection function is implemented. At the same time, lowering the confidence threshold can enhance the flexibility of detection to some extent, making the detection results more in line with actual requirements.

## Extension

How to convert gallery images to PixelMap format?

```typescript
  const photoPicker: photoAccessHelper.PhotoViewPicker = new photoAccessHelper.PhotoViewPicker();
  const photoPickerResult = await photoPicker.select({
      MIMEType: photoAccessHelper.PhotoViewMIMETypes.IMAGE_TYPE, maxSelectNumber: 1
    })
  const fileSource = await fileIo.open(name, fileIo.OpenMode.READ_ONLY);
  const imageSource = image.createImageSource(fileSource.fd);
  const chooseImage = await this.imageSource.createPixelMap();
  await fileIo.close(fileSource);
  // Steps:
  // 1. First, use photoPicker to select an image.
  // 2. Then, use fileIo.open to open the image file.
  // 3. Next, use image.createImageSource to create an image source.
  // 4. Finally, use the createPixelMap method to convert the image source to PixelMap format.
  // 5. After the conversion is complete, you can use the PixelMap - formatted image for skeleton detection.
```