# HarmonyOS 5 新闻应用新闻列表页实现案例

## 内容摘要
本文将介绍在 HarmonyOS 5.0 新闻应用里，使用 ArkTS 语言实现新闻列表页的方法。借助状态管理和 UI 组件，达成新闻分类筛选与列表展示功能。

## 实现步骤
1. 定义必要的状态变量。
2. 构建导航与搜索组件。
3. 实现新闻分类筛选功能。
4. 展示新闻列表。

## 落地代码
### 1. 定义状态变量
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
      author: '张三',
      company: '科技日报',
      time: '2025-06-20',
      title: 'HarmonyOS 5 新特性发布',
      category: '科技'
    },
    {
      id: '2',
      author: '李四',
      company: '体育新闻',
      time: '2025-06-21',
      title: '足球世界杯赛程公布',
      category: '体育'
    }
  ]
  @State categories: string[] = ['All', '科技', '体育', '财经', '世界']
}
```

### 2. 构建导航与搜索组件
```typescript
  build() {
    NavDestination() {
      Column() {
        Column() {
          Text('发现新闻')
            .fontSize(32)
            .fontWeight(FontWeight.Bold)
          Text('全球新闻一手掌握')
            .fontColor(Color.Gray)
            .margin({ top: 4 })
          Search({ placeholder: '搜索新闻' })
            .height(48)
            .searchButton('搜索')
            .margin({ top: 20 })
            .searchIcon({ size: 20 })
        }
        .width('100%')
        .padding(15)
        .alignItems(HorizontalAlign.Start)
```

### 3. 实现新闻分类筛选功能
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

### 4. 展示新闻列表
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

## 完整示例代码
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
      author: '张三',
      company: '科技日报',
      time: '2025-06-20',
      title: 'HarmonyOS 5 新特性发布',
      category: '科技'
    },
    {
      id: '2',
      author: '李四',
      company: '体育新闻',
      time: '2025-06-21',
      title: '足球世界杯赛程公布',
      category: '体育'
    }
  ]
  @State categories: string[] = ['All', '科技', '体育', '财经', '世界']

  build() {
    NavDestination() {
      Column() {
        Column() {
          Text('发现新闻')
            .fontSize(32)
            .fontWeight(FontWeight.Bold)
          Text('全球新闻一手掌握')
            .fontColor(Color.Gray)
            .margin({ top: 4 })
          Search({ placeholder: '搜索新闻' })
            .height(48)
            .searchButton('搜索')
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

## 总结
本文实现了 HarmonyOS 5 新闻应用的新闻列表页，关键知识点包括使用 `@State` 进行状态管理，`ForEach` 遍历列表，`List` 组件展示数据，以及实现新闻分类筛选功能。通过这些技术，能高效构建出功能丰富、交互性强的新闻列表页。