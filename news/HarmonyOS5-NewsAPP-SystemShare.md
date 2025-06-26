# HarmonyOS 5 新闻应用系统分享功能实现案例

## 内容摘要
本文详细介绍了在 HarmonyOS 5.0 新闻应用中实现系统分享功能的方法。借助 `BarButton` 组件和 `systemShare` 模块，实现了新闻链接的分享功能。

![效果图](img04.png)

## 实现步骤
1. 创建 `BarButton` 组件实例，设置分享图标。
2. 为 `BarButton` 组件添加点击事件。
3. 在点击事件中创建 `SharedData` 对象，设置分享数据。
4. 创建 `ShareController` 实例，传入分享数据。
5. 获取 UI 上下文，调用 `show` 方法显示分享界面。

## 落地代码
```typescript
BarButton({ icon: $r('sys.media.ohos_ic_public_share') }) 
         .onClick(() => { 
           // system share 
           const data = new systemShare.SharedData({ 
             utd: uniformTypeDescriptor.UniformDataType.HYPERLINK, 
             title: 'NewsAPP', 
             content: ' `https://edition.cnn.com/` ' 
           }) 
           const controller = new systemShare.ShareController(data) 
           const ctx = this.getUIContext().getHostContext() as common.UIAbilityContext 
           controller.show(ctx, { 
             previewMode: systemShare.SharePreviewMode.DETAIL, 
             selectionMode: systemShare.SelectionMode.SINGLE 
           }) 
         }) 
```

## 总结
本文实现了新闻应用的系统分享功能，关键知识点包括 `systemShare` 模块的使用、`SharedData` 对象和 `ShareController` 实例的创建，以及如何在点击事件中触发分享操作。通过这些步骤，可方便地在 HarmonyOS 应用中集成系统分享功能。