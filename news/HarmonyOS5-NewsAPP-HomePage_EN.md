# Case Study of Implementing the Home Page of a News App on HarmonyOS 5

## Abstract
This article introduces how to implement the home page of a news app on HarmonyOS 5. Using the provided ArkTS code, the home - page interface includes a carousel news display, a recommended news list, and supports navigating to the news list page when clicking "View all". An image `img01.png` is used as a screenshot to show the interface.

## Implementation Steps
1. Define the navigation path stack `pathStack`.
2. Define the news data model `NewsModel`.
3. Implement the `BannerNewsItem` component for carousel news display.
4. Implement the `ListNewsItem` component for the recommended news list display.
5. Implement the `TitleBar` component as the title bar, supporting navigation when clicking "View all".
6. Mock the news data `mockData`.
7. Implement the `PreviewPage` as the entry component, combining the above components to build the home - page interface.

## Code Implementation
```typescript
const pathStack = new NavPathStack() 

interface NewsModel { 
  id: string 
  author: string 
  company: string 
  companyLogo: ResourceStr
  time: string 
  title: string, 
  category: string, 
  cover: ResourceStr 
} 

@Component 
struct BannerNewsItem { 
  news: NewsModel = {} as NewsModel 

  build() { 
    Stack() { 
      Image( this .news.cover) 
        .width('100%') 
        .height('100%') 
      Column({ space: 8 }) { 
        Button( this .news.category) 
          .size({ height: 36 }) 
        Blank() 
        Text() { 
          Span( this .news.company) 
          Span( this .news.author) 
          Span('·') 
          Span( this .news.time) 
            .fontSize(12) 
        } 
        .fontColor(Color.White) 
        .fontWeight(FontWeight.Medium) 

        Text( this .news.title) 
          .fontSize(18) 
          .fontColor(Color.White) 
          .fontWeight(FontWeight.Bold) 
          .maxLines(2) 
          .textOverflow({ overflow: TextOverflow.Ellipsis }) 
      } 
      .padding(20) 
      .width('100%') 
      .height('100%') 
      .alignItems(HorizontalAlign.Start) 
    } 
    .width('100%') 
    .aspectRatio(5 / 3) 
    .borderRadius(20) 
    .clip( true ) 
  } 
} 

@Component 
struct ListNewsItem { 
  news: NewsModel = {} as NewsModel 

  build() { 
    Row({ space: 12 }) { 
      Image( this .news.cover) 
        .width(120) 
        .aspectRatio(1) 
        .objectFit(ImageFit.Cover) 
        .borderRadius(12) 
      Column({ space: 12 }) { 
        Text( this .news.category) 
          .fontColor(Color.Gray) 
          .fontSize(14) 
        Text( this .news.title) 
          .maxLines(2) 
          .textOverflow({ overflow: TextOverflow.Ellipsis }) 
          .fontWeight(500) 
        Text() { 
          Span( this .news.author) 
          Span('·') 
          Span( this .news.time) 
        } 
        .fontSize(14) 
        .fontColor(Color.Gray) 
      } 
      .width('100%') 
      .height(120) 
      .justifyContent(FlexAlign.Start) 
      .alignItems(HorizontalAlign.Start) 
      .layoutWeight(1) 
    } 
    .padding({ left: 15, right: 15 }) 
  } 
} 

@Component 
struct TitleBar { 
  title: string = '' 
  category: string = '' 

  build() { 
    Row() { 
      Text( this .title) 
        .fontSize(18) 
        .fontWeight(FontWeight.Bold) 
      Blank() 
      Button('View all') 
        .buttonStyle(ButtonStyleMode.TEXTUAL) 
        .fontSize(14) 
        .onClick(() => { 
          pathStack.pushPathByName('NewsListPage', this .category) 
        }) 
    } 
    .width('100%') 
    .padding({ left: 15, right: 15, bottom: 10 }) 
  } 
} 

const mockData: NewsModel[] = [ 
  { 
    id: '100', 
    author: 'Glen Levy', 
    company: 'CNN Indonesia',
    companyLogo: $r('app.media.news_cnn'),
    time: 'Fri June 20, 2025', 
    title: 'Lionel Messi: The player who’s bigger than the club and the FIFA Club World Cup tournament?', 
    cover: $r('app.media.news01'), 
    category: 'Sports' 
  }, 
  { 
    id: '101', 
    author: 'Glen Levy', 
    company: 'CNN Indonesia',
    companyLogo: $r('app.media.news_cnn'),
    time: 'Fri June 20, 2025', 
    title: 'Lionel Messi: The player who’s bigger than the club and the FIFA Club World Cup tournament?', 
    cover: $r('app.media.news01'), 
    category: 'Sports' 
  }, 
  { 
    id: '102', 
    author: 'Glen Levy', 
    company: 'CNN Indonesia',
    companyLogo: $r('app.media.news_cnn'),
    time: 'Fri June 20, 2025', 
    title: 'Lionel Messi: The player who’s bigger than the club and the FIFA Club World Cup tournament?', 
    cover: $r('app.media.news01'), 
    category: 'Sports' 
  } 
] 

@Entry 
@Component 
struct PreviewPage { 
  @State list: NewsModel[] = mockData 

  @Builder 
  PagesMap(name: string) { 
    if (name === 'NewsListPage') { 
      NewsListPage() 
    } 
  } 

  @Builder 
  BreakingNewsBuilder() { 
    Column() { 
      TitleBar({ category: 'All', title: 'Breaking News' }) 
      Swiper() { 
        ForEach( this .list, (item: NewsModel) => { 
          BannerNewsItem({ news: item }) 
        }) 
      } 
      .itemSpace(20) 
      .safeAreaPadding({ left: 15, right: 15, bottom: 40 }) 
    } 
  } 

  @Builder 
  RecommendNewsBuilder() { 
    Column() { 
      TitleBar({ category: 'All', title: 'Recommendation' }) 
      Column({ space: 12 }) { 
        ForEach( this .list, (item: NewsModel) => { 
          ListNewsItem({ news: item }) 
        }) 
      } 
    } 
  } 

  build() { 
    Navigation(pathStack) { 
      List() { 
        ListItem() { 
          this .BreakingNewsBuilder() 
        } 

        ListItem() { 
          this .RecommendNewsBuilder() 
        } 
      } 
      .width('100%') 
      .height('100%') 
    } 
    .mode(NavigationMode.Stack) 
    .navDestination( this .PagesMap) 
    .hideToolBar( true ) 
  } 
} 
```

## Screenshot
![Screenshot](img01.png)

## Conclusion
This case study implements the home - page interface of a news app on HarmonyOS 5 using the ArkTS language. It includes carousel news and recommended news list displays, as well as page navigation functionality. The key points include component definition and combination, the use of data models, and the implementation of navigation functions.