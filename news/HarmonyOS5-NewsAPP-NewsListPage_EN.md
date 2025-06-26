# HarmonyOS 5 News Application - News List Page Implementation Case

## Summary
This article details the implementation of the news list page in a HarmonyOS 5.0 news application using the ArkTS language. By defining state variables and building UI components, it achieves the functions of news category filtering and news list display.

## Implementation Steps
1. Define state variables `list` and `categories`.
2. Build the navigation destination component `NavDestination`.
3. Add a title, search box, and category list to the `Column` component.
4. Use the `List` component to display the news list.

## Code Implementation
```typescript
@Component 
struct NewsListPage { 
  @State list: NewsModel[] = mockData 
  @State categories: string[] = [ 
    'All', 
    'Sports', 
    'Politic', 
    'Business', 
    'World' 
  ] 

  build() { 
    NavDestination() { 
      Column() { 
        Column() { 
          Text('Discover') 
            .fontSize(32) 
            .fontWeight(FontWeight.Bold) 
          Text('News from all around the world') 
            .fontColor(Color.Gray) 
            .margin({ top: 4 }) 
          Search({ placeholder: 'News Keyword' }) 
            .height(48) 
            .searchButton('Search') 
            .margin({ top: 20 }) 
            .searchIcon({ size: 20 }) 
        } 
        .width('100%') 
        .padding(15) 
        .alignItems(HorizontalAlign.Start) 

        List({ space: 15 }) { 
          ForEach(this.categories, (cate: string) => { 
            ListItem() { 
              Button(cate) 
                .height(40) 
            } 
          }) 
        } 
        .contentStartOffset(15) 
        .width('100%') 
        .height(64) 
        .listDirection(Axis.Horizontal) 
        .scrollBar(BarState.Off) 

        List({ space: 12 }) { 
          ForEach(this.list, (item: NewsModel) => { 
            ListItem() { 
              ListNewsItem({ news: item }) 
            } 
          }) 
        } 
        .width('100%') 
        .height('100%') 
        .layoutWeight(1) 
      } 
      .width('100%') 
    } 
  } 
} 
```

## Conclusion
This article implements the core functions of the news list page in a news application, including state management, UI construction, and list display. Key knowledge points include using `@State` for state management, `ForEach` to iterate lists, and the `List` component to display data.

![Effect Diagram](img02.png)