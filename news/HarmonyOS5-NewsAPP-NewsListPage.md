# HarmonyOS 5 新闻应用新闻列表页实现案例

## 内容摘要
本文详细介绍了在 HarmonyOS 5.0 中使用 ArkTS 语言实现新闻应用新闻列表页的方法。通过定义状态变量和构建 UI 组件，实现了新闻分类筛选和新闻列表展示功能。

## 实现步骤
1. 定义状态变量 `list` 和 `categories`。
2. 构建导航目标组件 `NavDestination`。
3. 在 `Column` 组件中添加标题、搜索框和分类列表。
4. 使用 `List` 组件展示新闻列表。

## 落地代码
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

## 总结
本文实现了新闻应用新闻列表页的核心功能，包括状态管理、UI 构建和列表展示。关键知识点包括使用 `@State` 管理状态、`ForEach` 遍历列表、`List` 组件展示数据等。

![效果图](img02.png)