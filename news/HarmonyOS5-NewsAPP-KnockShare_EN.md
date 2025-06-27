# HarmonyOS 5 News Application - Knock Sharing Function Implementation Case

## Summary
This article details the implementation of the knock sharing function in a HarmonyOS 5.0 news application. The `KnockManager` class is used to manage knock sharing events and achieve the sharing of news content.

## Implementation Steps
1. Implement the `KnockManager` class using the singleton pattern to ensure there is only one instance globally.
2. Initialize the UI context and news data in the constructor.
3. Implement the `knockCallback` method to handle the specific logic of knock sharing, including obtaining media resources, saving files, and creating sharing data.
4. Implement the `bindEvent` and `unBindEvent` methods to bind and unbind knock sharing events.

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

## Conclusion
This article implements the knock sharing function of the news application. Key knowledge points include the application of the singleton pattern, file operations, media resource acquisition, and the creation of sharing data. The `KnockManager` class is used to centrally manage knock sharing events, which improves the maintainability and reusability of the code.