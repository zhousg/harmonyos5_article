# HarmonyOS 5 News App Calendar Account List Implementation

## Abstract
This article introduces the implementation of the `NewsLivePage` for the calendar account list in the HarmonyOS 5.0 news application. It uses the ArkTS language to implement functions such as calendar permission requests, calendar account creation, and list display.

## Technical Introduction
Calendar Kit (Calendar Service) provides calendar and schedule management capabilities, usually referring to APIs (Application Programming Interfaces) that can be used to access and manipulate calendar data. These interfaces allow developers to integrate time - related schedule services (such as travel, dining, sports, entertainment, etc.) from other applications into the system calendar, thereby achieving functions such as schedule management, event creation, and query.

Calendar accounts are used to store and manage personal or team schedules. Through calendar accounts, users can easily view, edit, and share schedule information. The CalendarManager is used to manage Calendar accounts. A calendar account mainly contains account information `CalendarAccount` and configuration information `CalendarConfig`.

Developers can create calendar accounts specific to their applications and perform operations such as adding, deleting, updating, and querying calendar accounts. In addition, each event `Event` belongs to a specific calendar account, and schedules under that account can be managed through the calendar account. For specific guidance, please refer to Schedule Management.

## Implementation Steps
1. Initialize the context and calendar manager.
2. Request calendar read and write permissions.
3. Implement the calendar account creation function.
4. Display the calendar account list.

## Code Implementation
### 1. Initialize the Context and Calendar Manager
```typescript
@Component 
struct NewsLivePage { 
  ctx = this.getUIContext().getHostContext() as common.UIAbilityContext 
  calendarMgr = calendarManager.getCalendarManager(this.ctx) 
  @State list: calendarManager.Calendar[] = [] 
}
```

### 2. Request Calendar Read and Write Permissions
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

### 3. Implement the Calendar Account Creation Function
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

### 4. Display the Calendar Account List
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

## Complete Code

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

## Summary
Key knowledge points include component state management with `@State`, permission requests, the use of asynchronous functions, and the use of the `List` and `ForEach` components. These implementations enable the creation and list display of calendar accounts.

## Note
- Ensure that the application has obtained calendar read and write permissions
- After permission requests, refresh the list display
- Set the background color of the list item based on the account color