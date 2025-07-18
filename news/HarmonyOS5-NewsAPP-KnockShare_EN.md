# HarmonyOS 5 News Application - Knock Sharing Function Implementation Case

## Summary
This article provides a detailed introduction to implementing the knock sharing function in a HarmonyOS 5.0 news application. The `KnockManager` class is used to manage knock sharing events and achieve the sharing of news content.

## Implementation Steps
1. Implement the `KnockManager` class using the singleton pattern to ensure there is only one instance globally.
```typescript
static getInstance(ctx: common.UIAbilityContext, news: NewsModel) {
  if (!KnockManager.instance) {
    KnockManager.instance = new KnockManager(ctx, news)
  }
  return KnockManager.instance
}
```
2. Initialize the UI context and news data in the constructor.
```typescript
constructor(ctx: common.UIAbilityContext, news: NewsModel) {
  this.ctx = ctx
}
```
3. Implement the `knockCallback` method to handle the specific logic of knock sharing, including obtaining media resources, saving files, and creating sharing data.
```typescript
knockCallback(target: harmonyShare.SharableTarget) {
  if (this.news && this.ctx) {
    // The demo cover is a media resource, write it to the sandbox.
    const media = this.ctx.resourceManager.getMediaContentSync(this.news.cover as Resource)
    const filePath = this.ctx.filesDir + '/share_' + Date.now() + '.png'
    const file = fileIo.openSync(filePath, fileIo.OpenMode.READ_WRITE | fileIo.OpenMode.CREATE)
    fileIo.writeSync(file.fd, media.buffer)
    const uri = fileUri.getUriFromPath(filePath)
    // Create share data
    const sharaData: systemShare.SharedData = new systemShare.SharedData({
      utd: uniformTypeDescriptor.UniformDataType.HYPERLINK,
      content: ' `https://edition.cnn.com/2025/06/20/sport/lionel-messi-club-world-cup-inter-miami-spt` ',
      thumbnailUri: uri,
      title: this.news.title,
      description: this.news.company,
    })
    // Perform knock share
    target.share(sharaData)
  }
}
```
4. Implement the `bindEvent` and `unBindEvent` methods to bind and unbind knock sharing events.
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

## Code Implementation
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

   // Handle knock logic
   knockCallback(target: harmonyShare.SharableTarget) {
     if (this.news && this.ctx) {
       // The demo cover is a media resource, write it to the sandbox.
       const media = this.ctx.resourceManager.getMediaContentSync(this.news.cover as Resource)
       const filePath = this.ctx.filesDir + '/share_' + Date.now() + '.png'
       const file = fileIo.openSync(filePath, fileIo.OpenMode.READ_WRITE | fileIo.OpenMode.CREATE)
       fileIo.writeSync(file.fd, media.buffer)
       const uri = fileUri.getUriFromPath(filePath)
       // Create share data
       const sharaData: systemShare.SharedData = new systemShare.SharedData({
         utd: uniformTypeDescriptor.UniformDataType.HYPERLINK,
         content: ' `https://edition.cnn.com/2025/06/20/sport/lionel-messi-club-world-cup-inter-miami-spt` ',
         thumbnailUri: uri,
         title: this.news.title,
         description: this.news.company,
       })
       // Perform knock share
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

## Conclusion
This article implements the knock sharing function of the news application. Key knowledge points include the application of the singleton pattern, file operations, media resource acquisition, and the creation of sharing data. The `KnockManager` class is used to centrally manage knock sharing events, which improves the maintainability and reusability of the code.

## Use Cases
The host application enters a shareable interface, such as opening or selecting a file, a memo, a contact detail, or personal hotspot/Wi-Fi.
The host application can share multiple items, such as multiple selected images.
Share Kit supports tap - to - share between mobile phones and PC/2in1 devices. Leveraging the screen perception capability of PC/2in1 devices, it can recognize the action and position of a mobile phone tapping the screen to achieve window - level interaction on PC/2in1 devices.