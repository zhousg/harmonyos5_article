# 骨骼检测案例文章

## 内容摘要
本文主要介绍在HarmonyOS 5.0中使用ArkTS语言实现骨骼检测的案例。通过使用 `visionBase` 和 `skeletonDetector` 进行骨骼关键点的检测，同时降低了置信度要求以提高检测的灵活性。

## 适用场景

人体骨骼关键点检测，主要检测人体的一些关键点，通过关键点描述人体骨骼信息。具体应用主要集中在智能视频监控，病人监护系统，人机交互，虚拟现实，人体动画，智能家居，智能安防，运动员辅助训练等等。

支持17个关键点的识别，具体为鼻子，左右眼，左右耳，左右肩，左右肘、左右手腕、左右髋、左右膝、左右脚踝。

## 实现步骤
1. 定义请求对象。
2. 调用骨骼检测处理方法。
3. 提取关键点数据。
4. 根据类型获取特定关键点。

## 落地代码
### 1. 定义请求对象
```typescript
const request: visionBase.Request = {
  inputData: { pixelMap }
}
```

### 2. 调用骨骼检测处理方法
```typescript
const data = await skeletonDetector.process(request)
```

### 3. 提取关键点数据
```typescript
const points = data.skeletons[0].points
```

### 4. 降低置信度要求并定义获取关键点的方法
```typescript
const MIN_CONFIDENCE = 0.3; // 降低置信度要求
const getPoint = (type: skeletonDetection.SkeletonPointType) => points.find(p => p.type === type && p.score >= MIN_CONFIDENCE)?.point;
```

### 5. 获取特定关键点
```typescript
// 获取关键点（优先使用鼻子，其次用耳朵）
const nose = getPoint(skeletonDetection.SkeletonPointType.NOSE);
const leftShoulder = getPoint(skeletonDetection.SkeletonPointType.LEFT_SHOULDER);
const rightShoulder = getPoint(skeletonDetection.SkeletonPointType.RIGHT_SHOULDER);
let leftEar = getPoint(skeletonDetection.SkeletonPointType.LEFT_EAR);
let rightEar = getPoint(skeletonDetection.SkeletonPointType.RIGHT_EAR);
```

## 约束限制

输入图像具有合适成像的质量（建议720p以上），100px<高度<10000px，100px<宽度<10000px，高宽比例建议5:1以下（高度小于宽度的5倍），接近手机屏幕高宽比例为宜。

## 总结
本次案例的关键知识点在于如何在HarmonyOS 5.0中使用ArkTS语言进行骨骼检测。通过定义请求对象、调用处理方法和提取关键点数据，实现了基本的骨骼检测功能。同时，降低置信度要求可以在一定程度上提高检测的灵活性，使检测结果更加符合实际需求。