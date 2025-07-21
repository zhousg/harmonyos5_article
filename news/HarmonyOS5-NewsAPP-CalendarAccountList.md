# HarmonyOS5 新闻应用日历账户列表实现

## 内容摘要
本文介绍 HarmonyOS 5.0 新闻应用中日历账户列表页面 `NewsLivePage` 的实现方法，使用 ArkTS 语言实现日历权限请求、日历账户创建和列表展示等功能。

## 技术介绍

Calendar Kit（日历服务）提供日历与日程管理能力，通常是指可以用于访问和操作日历数据的API（应用程序接口）。这些接口允许开发者将其他应用中的工作、生活中与时间相关的日程服务（如出行、餐饮、运动、娱乐等）与系统日历进行集成，从而实现日程管理、事件创建、查询等功能。

日历账户‌用于存储和管理个人或团队的日程，通过日历账户，用户可以方便地查看、编辑和共享日程信息。日历管理器CalendarManager用于管理日历账户Calendar。日历账户主要包含账户信息CalendarAccount和配置信息CalendarConfig。
开发者可以创建属于应用特有的日历账户，还可以对日历账户进行新增、删除、更新和查询。此外，每个日程Event归属于某一个特定的日历账户，可以通过日历账户对该账户下面的日程进行管理，具体相关指导可见日程管理。


## 实现步骤
1. 初始化上下文和日历管理器
2. 请求日历读写权限
3. 实现创建日历账户功能
4. 展示日历账户列表

## 落地代码
### 1. 初始化上下文和日历管理器
```typescript
@Component 
struct NewsLivePage { 
  ctx = this.getUIContext().getHostContext() as common.UIAbilityContext 
  calendarMgr = calendarManager.getCalendarManager(this.ctx) 
  @State list: calendarManager.Calendar[] = [] 
}
```

### 2. 请求日历读写权限
```typescript
aboutToAppear(): void { 
  const permissions: Permissions[] = [ 
    'ohos.permission.READ_CALENDAR', 
    'ohos.permission.WRITE_CALENDAR' 
  ]; 
  let atManager = abilityAccessCtrl.createAtManager() 
  atManager.requestPermissionsFromUser(this.ctx, permissions) 
}
```

### 3. 实现创建日历账户功能
```typescript
async addAccount (name: string, color: string) { 
  const calendar3 = await this.calendarMgr.createCalendar({ 
    name: name, 
    type: calendarManager.CalendarType.LOCAL, 
    displayName: name + ' Account' 
  }) 
  calendar3.setConfig({ 
    enableReminder: true, 
    color: color 
  }); 
}
```

### 4. 展示日历账户列表
```typescript
build() { 
  NavDestination(){ 
    Stack({ alignContent: Alignment.Bottom }){ 
      List({ space: 15 }){ 
        ForEach(this.list, (calendar: calendarManager.Calendar) => { 
          ListItem(){ 
            Row(){ 
              Text(calendar.getAccount().displayName) 
                .fontSize(24) 
            } 
            .padding(15) 
            .width('100%') 
            .height(120) 
            .borderRadius(20) 
            .backgroundColor(calendar.getConfig().color) 
          } 
        }) 
      } 
      .padding(15) 
      .width('100%') 
      .height('100%') 
    } 
  } 
  .title('Calendar Account') 
  .onReady(async ctx => { 
    this.list = await this.calendarMgr.getAllCalendars() 
  }) 
}
```

## 完整代码

```typescript
@Component 
struct NewsLivePage { 
  ctx = this.getUIContext().getHostContext() as common.UIAbilityContext 
  calendarMgr = calendarManager.getCalendarManager(this.ctx) 
  @State list: calendarManager.Calendar[] = [] 

  aboutToAppear(): void { 
    const permissions: Permissions[] = [ 
      'ohos.permission.READ_CALENDAR', 
      'ohos.permission.WRITE_CALENDAR' 
    ]; 
    let atManager = abilityAccessCtrl.createAtManager() 
    atManager.requestPermissionsFromUser(this.ctx, permissions) 
  }

  build() { 
  NavDestination(){ 
    Stack({ alignContent: Alignment.Bottom }){ 
      List({ space: 15 }){ 
        ForEach(this.list, (calendar: calendarManager.Calendar) => { 
          ListItem(){ 
            Row(){ 
              Text(calendar.getAccount().displayName) 
                .fontSize(24) 
            } 
            .padding(15) 
            .width('100%') 
            .height(120) 
            .borderRadius(20) 
            .backgroundColor(calendar.getConfig().color) 
          } 
        }) 
      } 
      .padding(15) 
      .width('100%') 
      .height('100%') 
    } 
  } 
  .title('Calendar Account') 
  .onReady(async ctx => { 
    this.list = await this.calendarMgr.getAllCalendars() 
  }) 
}
}
```

## 总结
关键知识点包括组件状态管理 `@State`、权限请求、异步函数使用以及列表组件 `List` 和 `ForEach` 的使用。通过这些实现了日历账户的创建和列表展示功能。


## 注意事项
- 确保应用已获取日历读写权限
- 权限请求后需要刷新列表展示
- 列表展示时需要根据账户颜色设置背景