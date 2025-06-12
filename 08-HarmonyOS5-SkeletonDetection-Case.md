# Skeleton Detection Case Article

## Summary
This article mainly introduces a case of implementing skeleton detection using the ArkTS language in HarmonyOS 5.0. By using `visionBase` and `skeletonDetector`, the detection of skeletal key points is performed. Meanwhile, the confidence threshold is lowered to enhance the flexibility of detection.

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

## Conclusion
The key knowledge point of this case lies in how to perform skeleton detection using the ArkTS language in HarmonyOS 5.0. By defining the request object, calling the processing method, and extracting key point data, the basic skeleton detection function is implemented. At the same time, lowering the confidence threshold can enhance the flexibility of detection to some extent, making the detection results more in line with actual requirements.