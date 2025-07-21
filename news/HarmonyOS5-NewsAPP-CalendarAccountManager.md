# HarmonyOS5 新闻应用日历账户管理实现

## 内容摘要
本文介绍 HarmonyOS 5.0 新闻应用中日历账户管理功能的实现方法，包含添加特定类型日历账户的按钮和删除日历账户的构建器，使用 ArkTS 语言实现账户的增删操作。

## 实现步骤
1. 添加账户按钮组件实现
2. 删除账户构建器实现
3. 绑定账户增删操作逻辑

## 落地代码
### 1. 添加账户按钮组件实现
```typescript
Column({ space: 15 }){ 
  Button('Add Sports News Account') 
    .onClick(async () => { 
      await this.addAccount('Sports News', '#006699') 
      this.list = await this.calendarMgr.getAllCalendars() 
    }) 
  Button('Add World News Account') 
    .onClick(async () => { 
      await this.addAccount('World News', '#ffaa00') 
      this.list = await this.calendarMgr.getAllCalendars() 
    }) 
} 
.margin(15) 
```

### 2. 删除账户构建器实现
```typescript
@Builder 
DeleteBuilder (calendar: calendarManager.Calendar) { 
  Text('删除') 
    .fontSize(24) 
    .width(120) 
    .textAlign(TextAlign.Center) 
    .height('100%') 
    .onClick(async () => { 
      await this.calendarMgr.deleteCalendar(calendar) 
      this.list = await this.calendarMgr.getAllCalendars() 
    }) 
} 
```

### 3. 操作逻辑说明
在点击添加账户按钮时，调用 `addAccount` 方法创建对应类型的日历账户，随后更新账户列表。点击删除按钮时，调用 `deleteCalendar` 方法删除指定账户，同样更新账户列表。

## 其他API说明

getCalendarManager(context: Context): CalendarManager	根据上下文获取日历管理器对象CalendarManager，用于管理日历。

createCalendar(calendarAccount: CalendarAccount): Promise<Calendar>	根据日历账户信息，创建一个Calendar对象，使用Promise异步回调。

getCalendar(calendarAccount?: CalendarAccount): Promise<Calendar>	
获取默认Calendar对象或者指定Calendar对象，使用Promise异步回调。

默认Calendar是日历存储首次运行时创建的，若创建Event时不关注其Calendar归属，则无须通过createCalendar()创建Calendar，直接使用默认Calendar。

getAllCalendars(): Promise<Calendar[]>	获取当前应用所有创建的Calendar对象以及默认Calendar对象，使用Promise异步回调。

deleteCalendar(calendar: Calendar): Promise<void>	删除指定Calendar对象，使用Promise异步回调。

getConfig(): CalendarConfig	获取日历配置信息。

setConfig(config: CalendarConfig): Promise<void>	设置日历配置信息，使用Promise异步回调。

getAccount(): CalendarAccount	获取日历账户信息。


## 总结
关键知识点包括 `@Builder` 装饰器的使用、按钮点击事件处理、异步函数调用以及状态更新。通过这些实现了日历账户的添加和删除功能，并且每次操作后都会更新账户列表展示。

## 适用场景

Calendar Kit（日历服务）提供日历与日程管理能力，通常是指可以用于访问和操作日历数据的API（应用程序接口）。这些接口允许开发者将其他应用中的工作、生活中与时间相关的日程服务（如出行、餐饮、运动、娱乐等）与系统日历进行集成，从而实现日程管理、事件创建、查询等功能。

日程一键服务：开发者通过永久性授权机制，在用户许可当前应用读写系统日历后，可将对应的日程服务以deeplink的形式同时写入日历。根据开发者设置的提醒规则，在日程临近或到期时，日历应用、日历通知、日历卡片等区域同时会显示对应的“服务按钮”，用户可通过点击此按钮拉起跳转链接，一步直达服务落地页。

## 约束限制

目前Calendar Kit的相关能力及接口使用，仅支持在Stage模型下使用。

使用Calendar Kit的相关能力，需要获取读取或写入日历、日程的权限。具体指导可见声明权限。

进行日历或日程的读取时，需要申请ohos.permission.READ_CALENDAR权限。

进行日历或日程的添加、删除或修改时，需要申请ohos.permission.WRITE_CALENDAR权限。
