# HarmonyOS 5 新闻应用敲击分享功能实现案例

## 内容摘要
本文详细介绍了在 HarmonyOS 5.0 新闻应用中实现敲击分享功能的方法。通过 `KnockManager` 类对敲击分享事件进行管理，实现新闻内容的分享操作。

## 实现步骤
1. 实现单例模式的 `KnockManager` 类，保证全局只有一个实例。
```typescript
static getInstance(ctx: common.UIAbilityContext, news: NewsModel) {
  if (!KnockManager.instance) {
    KnockManager.instance = new KnockManager(ctx, news)
  }
  return KnockManager.instance
}
```
2. 在构造函数中初始化 UI 上下文和新闻数据。
```typescript
constructor(ctx: common.UIAbilityContext, news: NewsModel) {
  this.ctx = ctx
}
```
3. 实现 `knockCallback` 方法，处理敲击分享的具体逻辑，包括获取媒体资源、保存文件、创建分享数据等。
```typescript
knockCallback(target: harmonyShare.SharableTarget) {
  if (this.news && this.ctx) {
    // demo cover is media resoure, write in sandbox
    const media = this.ctx.resourceManager.getMediaContentSync(this.news.cover as Resource)
    const filePath = this.ctx.filesDir + '/share_' + Date.now() + '.png'
    const file = fileIo.openSync(filePath, fileIo.OpenMode.READ_WRITE | fileIo.OpenMode.CREATE)
    fileIo.writeSync(file.fd, media.buffer)
    const uri = fileUri.getUriFromPath(filePath)
    // create share data
    const sharaData: systemShare.SharedData = new systemShare.SharedData({
      utd: uniformTypeDescriptor.UniformDataType.HYPERLINK,
      content: ' `https://edition.cnn.com/2025/06/20/sport/lionel-messi-club-world-cup-inter-miami-spt` ',
      thumbnailUri: uri,
      title: this.news.title,
      description: this.news.company,
    })
    // knock share
    target.share(sharaData)
  }
}
```
4. 实现 `bindEvent` 和 `unBindEvent` 方法，用于绑定和解绑敲击分享事件。
```typescript
bindEvent() {
  if (!this.isBind) {
    harmonyShare.on('knockShare', (target) => {
      this.knockCallback(target)
    })
    this.isBind = true
  }
}

unBindEvent() {
  harmonyShare.off('knockShare')
  this.isBind = false
}
```

## 落地代码
```typescript
export class KnockManager { 
   private static instance: KnockManager 
   private ctx?: common.UIAbilityContext 
   private isBind: boolean = false 
   private news?: NewsModel 

   static getInstance(ctx: common.UIAbilityContext, news: NewsModel) { 
     if (!KnockManager.instance) { 
       KnockManager.instance = new KnockManager(ctx, news) 
     } 
     return KnockManager.instance 
   } 

   constructor(ctx: common.UIAbilityContext, news: NewsModel) { 
     this.ctx = ctx 
   } 

   // handle knock logic 
   knockCallback(target: harmonyShare.SharableTarget) { 
     if (this.news && this.ctx) { 
       // demo cover is media resoure, write in sandbox 
       const media = this.ctx.resourceManager.getMediaContentSync(this.news.cover as Resource) 
       const filePath = this.ctx.filesDir + '/share_' + Date.now() + '.png' 
       const file = fileIo.openSync(filePath, fileIo.OpenMode.READ_WRITE | fileIo.OpenMode.CREATE) 
       fileIo.writeSync(file.fd, media.buffer) 
       const uri = fileUri.getUriFromPath(filePath) 
       // create share data 
       const sharaData: systemShare.SharedData = new systemShare.SharedData({ 
         utd: uniformTypeDescriptor.UniformDataType.HYPERLINK, 
         content: ' `https://edition.cnn.com/2025/06/20/sport/lionel-messi-club-world-cup-inter-miami-spt` ', 
         thumbnailUri: uri, 
         title: this.news.title, 
         description: this.news.company, 
       }) 
       // knock share 
       target.share(sharaData) 
     } 
   } 

   bindEvent() { 
     if (!this.isBind) { 
       harmonyShare.on('knockShare', (target) => { 
         this.knockCallback(target) 
       }) 
       this.isBind = true 
     } 
   } 

   unBindEvent() { 
     harmonyShare.off('knockShare') 
     this.isBind = false 
   } 
 } 
```

## 总结
本文实现了新闻应用的敲击分享功能，关键知识点包括单例模式的应用、文件操作、媒体资源获取以及分享数据的创建。通过 `KnockManager` 类，对敲击分享事件进行统一管理，提高了代码的可维护性和复用性。


## 使用场景

宿主应用进入一个可以分享的界面，比如打开或者选中的一个文件、一条备忘录、一个联系人详情，或个人热点/Wi-Fi等。
宿主应用可以分享多个内容，如选中的多张图片等。
Share Kit支持手机和PC/2in1之间的碰一碰分享。利用PC/2in1设备的屏幕感知能力，识别手机轻碰屏幕的动作及位置，实现PC/2in1窗口级的交互。
