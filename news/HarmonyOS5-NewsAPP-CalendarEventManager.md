# HarmonyOS5 新闻应用日历事件管理实现

## 内容摘要
本文介绍 HarmonyOS 5.0 新闻应用中日历事件管理功能的实现方法，包含绑定弹窗、添加日历事件和展示事件详情等功能，使用 ArkTS 语言实现。

## 实现步骤
1. 绑定弹窗组件
2. 实现添加日历事件功能
3. 构建账户详情组件展示事件列表

## 落地代码
### 1. 绑定弹窗组件
```typescript
.bindSheet($$this.isShow, this.AccountDetailBuilder, { 
  height: 500 
}) 
```

### 2. 实现添加日历事件功能
```typescript
async addCalendarEvent (title: string) { 
  await this.currCalendar?.addEvent({ 
    title: title, 
    startTime: Number(new Date('2025-12-12 19:00:00')), 
    endTime: Number(new Date('2025-12-12 21:00:00')), 
    reminderTime: [0, 5, 10, 60], 
    type: calendarManager.EventType.NORMAL 
  }) 
  this.currCalendarEvents = await this.currCalendar?.getEvents() || [] 
}
```

### 3. 构建账户详情组件展示事件列表
```typescript
@Builder 
AccountDetailBuilder() { 
  Column({ space: 15 }) { 
    Text(this.currCalendar?.getAccount().displayName) 
      .fontSize(24) 
      .fontWeight(500) 
    Column({ space: 15 }) { 
      Button('Subscribe CNN News') 
        .onClick(() => { 
          this.addCalendarEvent('Subscribe CNN News') 
        }) 
      Button('Subscribe BBC News') 
        .onClick(() => { 
          this.addCalendarEvent('Subscribe BBC News') 
        }) 
    } 
    Text('My Subscribe : ') 
    List({ space: 15 }) { 
      ForEach(this.currCalendarEvents, (event: calendarManager.Event) => { 
        ListItem(){ 
          Column({ space: 5 }){ 
            Text(event.title) 
            Text(new Date(event.startTime).toString()) 
          } 
          .padding({ left: 15, right: 15 }) 
          .justifyContent(FlexAlign.Center) 
          .alignItems(HorizontalAlign.Start) 
          .width('100%') 
          .height(60) 
          .borderRadius(12) 
          .backgroundColor('#DDDDDD') 
        } 
      }) 
    } 
  } 
  .padding(15) 
  .alignItems(HorizontalAlign.Start) 
}
```

![img06.png](img06.png)

## 总结
关键知识点包括弹窗绑定、异步函数调用、事件添加和列表展示。通过 `addCalendarEvent` 方法添加事件，`AccountDetailBuilder` 组件展示订阅的新闻事件，借助 `List` 和 `ForEach` 组件展示事件列表。