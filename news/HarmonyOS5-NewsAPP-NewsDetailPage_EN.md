# HarmonyOS 5 News Application - News Detail Page Implementation Case

## Summary
This article details the implementation of the news detail page in a HarmonyOS 5.0 news application using the ArkTS language. By defining the `BarButton` and `NewDetailPage` components, it implements the interface layout, status bar color setting, and page navigation of the news detail page.

## Implementation Steps
1. Define the `BarButton` component to display icon buttons.
2. Define the `NewDetailPage` component to handle the business logic of the news detail page.
3. Implement the `setStatusBarContentColor` method to set the status bar color.
4. Build three `@Builder` functions, `CustomBarBuilder`, `TitleBuilder`, and `ContentBuilder`, to construct the UI of different parts of the page.
5. Combine the components in the `build` method to complete the page layout.

## Code Implementation
```typescript
@Component 
struct BarButton { 
  icon: ResourceStr = '' 

  build() { 
    Row() { 
      Image(this.icon) 
        .width(24) 
        .height(24) 
        .fillColor(Color.White) 
    } 
    .justifyContent(FlexAlign.Center) 
    .width(40) 
    .aspectRatio(1) 
    .borderRadius(22) 
    .backgroundColor('#45FFFFFF') 
  } 
} 

@Component 
struct NewDetailPage { 
  news: NewsModel = {} as NewsModel 


  async setStatusBarContentColor(color: string) { 
    const ctx = this.getUIContext() 
      .getHostContext()! 
    const win = await window.getLastWindow(ctx) 
    win.setWindowSystemBarProperties({ 
      statusBarContentColor: color 
    }) 
  } 

  @Builder 
  CustomBarBuilder() { 
    Row({ space: 10 }) { 
      BarButton({ icon: $r('sys.media.ohos_ic_public_arrow_left') }) 
        .onClick(() => pathStack.pop()) 
      Blank() 
      BarButton({ icon: $r('sys.media.ohos_ic_public_share') }) 
      BarButton({ icon: $r('sys.media.ohos_ic_public_more') }) 
    } 
    .padding(15) 
    .width('100%') 
  } 

  @Builder 
  TitleBuilder () { 
    Column({ space: 12 }){ 
      Button(this.news.category) 
        .size({ height: 36 }) 
      Text(this.news.title) 
        .fontSize(24) 
        .fontWeight(FontWeight.Medium) 
        .fontColor(Color.White) 
      Text() { 
        Span(this.news.author) 
        Span('·') 
        Span(this.news.time) 
      } 
      .fontSize(14) 
      .fontColor(Color.White) 
    } 
    .padding(15) 
    .height(300) 
    .width('100%') 
    .justifyContent(FlexAlign.End) 
    .alignItems(HorizontalAlign.Start) 
  } 

  @Builder 
  ContentBuilder () { 
    Column(){ 
      Row({ space: 10 }){ 
        Image(this.news.companyLogo) 
          .width(40) 
          .aspectRatio(1) 
          .borderRadius(20) 
        Text(this.news.company) 
          .fontSize(18) 
          .fontWeight(FontWeight.Bold) 
      } 
      .width('100%') 
      .height(60) 
      Text(` 
      At the newly expanded FIFA Club World Cup on Thursday in Atlanta, with the event still in its infancy, that old saying was tossed to the side and replaced by a question on the lips of the 31,783 in attendance – can a player be bigger than the tournament? 

      The soccer star in question, of course, was a certain Lionel Andrés Messi, for whom the masses had made their pilgrimage to worship, as his Inter Miami took on Portuguese giant FC Porto in the second matchday of Group A. 

      But until a magical Messi moment in the 54th minute, the match was in danger of becoming a mere sideshow to supporters expressing their admiration – actually, more like unbridled passion – for the 37-year-old who has long cemented his status as one of the greats of the sport. And let’s face it: winning nearly everything of note for Barcelona and Paris Saint-Germain – as well as his country of Argentina, who is the reigning World Cup champion – doesn’t exactly hurt his case. 
      `) 
        .fontSize(16) 
        .lineHeight(24) 

    } 
    .borderRadius({ topLeft: 30, topRight: 30 }) 
    .backgroundColor(Color.White) 
    .padding(15) 
  } 

  build() { 
    NavDestination() { 
      List(){ 
        ListItem(){ 
          this.CustomBarBuilder() 
        } 
        ListItem(){ 
          this.TitleBuilder() 
        } 
        ListItem(){ 
          this.ContentBuilder() 
        } 
      } 
      .width('100%') 
      .height('100%') 
      .layoutWeight(1) 
    } 
    .hideTitleBar(true) 
    .backgroundImage($r('app.media.news01')) 
    .backgroundImageSize({ height: '60%', width: 'auto' }) 
    .backgroundImagePosition(Alignment.Top) 
    .onShown(() => this.setStatusBarContentColor('#FFFFFF')) 
    .onHidden(() => this.setStatusBarContentColor('#000000')) 
    .onReady((ctx) => { 
      this.news = ctx.pathInfo.param as NewsModel 
    }) 
  } 
} 
```

## Conclusion
This article implements the core functions of the news detail page in a news application. Key knowledge points include component - based development, the use of `@Builder` functions, dynamic status bar color setting, and page navigation. Component - based development improves the maintainability and reusability of the code.

![Effect Diagram](img03.png)