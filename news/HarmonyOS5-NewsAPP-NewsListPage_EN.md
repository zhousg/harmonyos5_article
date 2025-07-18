# HarmonyOS 5 News Application - News List Page Implementation Case

## Summary
This article introduces how to implement the news list page in a HarmonyOS 5.0 news application using the ArkTS language. By leveraging state management and UI components, it achieves the functions of news category filtering and list display.

## Implementation Steps
1. Define necessary state variables.
2. Build navigation and search components.
3. Implement the news category filtering function.
4. Display the news list.

## Code Implementation
### 1. Define State Variables
```typescript
interface NewsModel {
  id: string
  author: string
  company: string
  time: string
  title: string
  category: string
}

@Component
struct NewsListPage {
  @State list: NewsModel[] = [
    {
      id: '1',
      author: 'Zhang San',
      company: 'Science and Technology Daily',
      time: '2025-06-20',
      title: 'HarmonyOS 5 New Features Released',
      category: 'Technology'
    },
    {
      id: '2',
      author: 'Li Si',
      company: 'Sports News',
      time: '2025-06-21',
      title: 'Football World Cup Schedule Announced',
      category: 'Sports'
    }
  ]
  @State categories: string[] = ['All', 'Technology', 'Sports', 'Finance', 'World']
}
```

### 2. Build Navigation and Search Components
```typescript
  build() {
    NavDestination() {
      Column() {
        Column() {
          Text('Discover News')
            .fontSize(32)
            .fontWeight(FontWeight.Bold)
          Text('Stay Informed with Global News')
            .fontColor(Color.Gray)
            .margin({ top: 4 })
          Search({ placeholder: 'Search News' })
            .height(48)
            .searchButton('Search')
            .margin({ top: 20 })
            .searchIcon({ size: 20 })
        }
        .width('100%')
        .padding(15)
        .alignItems(HorizontalAlign.Start)
```

### 3. Implement the News Category Filtering Function
```typescript
        List({ space: 15 }) {
          ForEach(this.categories, (cate: string) => {
            ListItem() {
              Button(cate)
                .height(40)
                .onClick(() => {
                  if (cate === 'All') {
                    this.list = mockData
                  } else {
                    this.list = mockData.filter(item => item.category === cate)
                  }
                })
            }
          })
        }
        .contentStartOffset(15)
        .width('100%')
        .height(64)
        .listDirection(Axis.Horizontal)
        .scrollBar(BarState.Off)
```

### 4. Display the News List
```typescript
        List({ space: 12 }) {
          ForEach(this.list, (item: NewsModel) => {
            ListItem() {
              Column({ space: 8 }) {
                Text(item.title)
                  .fontSize(18)
                  .fontWeight(FontWeight.Medium)
                Row() {
                  Text(item.company)
                    .fontSize(14)
                    .fontColor(Color.Gray)
                  Text(item.time)
                    .fontSize(14)
                    .fontColor(Color.Gray)
                    .margin({ left: 10 })
                }
              }
              .padding(15)
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
```

## Complete Example Code
```typescript
interface NewsModel {
  id: string
  author: string
  company: string
  time: string
  title: string
  category: string
}

@Component
struct NewsListPage {
  @State list: NewsModel[] = [
    {
      id: '1',
      author: 'Zhang San',
      company: 'Science and Technology Daily',
      time: '2025-06-20',
      title: 'HarmonyOS 5 New Features Released',
      category: 'Technology'
    },
    {
      id: '2',
      author: 'Li Si',
      company: 'Sports News',
      time: '2025-06-21',
      title: 'Football World Cup Schedule Announced',
      category: 'Sports'
    }
  ]
  @State categories: string[] = ['All', 'Technology', 'Sports', 'Finance', 'World']

  build() {
    NavDestination() {
      Column() {
        Column() {
          Text('Discover News')
            .fontSize(32)
            .fontWeight(FontWeight.Bold)
          Text('Stay Informed with Global News')
            .fontColor(Color.Gray)
            .margin({ top: 4 })
          Search({ placeholder: 'Search News' })
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
                .onClick(() => {
                  if (cate === 'All') {
                    this.list = mockData
                  } else {
                    this.list = mockData.filter(item => item.category === cate)
                  }
                })
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
              Column({ space: 8 }) {
                Text(item.title)
                  .fontSize(18)
                  .fontWeight(FontWeight.Medium)
                Row() {
                  Text(item.company)
                    .fontSize(14)
                    .fontColor(Color.Gray)
                  Text(item.time)
                    .fontSize(14)
                    .fontColor(Color.Gray)
                    .margin({ left: 10 })
                }
              }
              .padding(15)
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
This article implements the news list page of a HarmonyOS 5 news application. Key knowledge points include using `@State` for state management, `ForEach` to iterate lists, the `List` component to display data, and implementing the news category filtering function. With these techniques, you can efficiently build a feature - rich and interactive news list page.

![Effect Diagram](img02.png)