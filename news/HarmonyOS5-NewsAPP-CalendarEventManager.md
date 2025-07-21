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


## 其他API说明

getCalendarManager(context: Context): CalendarManager	根据上下文获取CalendarManager对象，用于管理日历。

createCalendar(calendarAccount: CalendarAccount): Promise<Calendar>	根据日历账户信息，创建一个Calendar对象，使用Promise异步回调。

addEvent(event: Event): Promise<number>	创建日程，入参Event不填日程id，使用Promise异步回调。

editEvent(event: Event): Promise<number>	创建单个日程，入参Event不填日程id，调用该接口会跳转到日程创建页面，使用Promise异步回调。

deleteEvent(id: number): Promise<void>	删除指定日程id的日程，使用Promise异步回调。

updateEvent(event: Event): Promise<void>	更新日程，使用Promise异步回调。

getEvents(eventFilter?: EventFilter, eventKey?: (keyof Event)[]): Promise<Event[]>	获取Calendar下符合查询条件的Event，使用Promise异步回调。

## 总结
关键知识点包括弹窗绑定、异步函数调用、事件添加和列表展示。通过 `addCalendarEvent` 方法添加事件，`AccountDetailBuilder` 组件展示订阅的新闻事件，借助 `List` 和 `ForEach` 组件展示事件列表。


## 注意

日程指特定的事件或者活动安排，日程管理即对这些事件、活动进行规划和控制，能更有效地利用相关资源、提高生产力和效率，使人们更好地管理时间和任务。

Calendar Kit中的日程Event归属于某个对应的日历账户Calendar，一个日历账户下可以有多个日程，一个日程只属于一个Calendar。

获取到日历账户对象之后，即可对该账户下的日程进行管理，包括日程的创建、删除、修改、查询等操作。在创建、修改日程时，支持对日程的标题、开始时间、结束时间、日程类型、日程地点、日程提醒时间、日程重复规则等相关信息进行设置，以便进行更丰富更有效的日程管理。

