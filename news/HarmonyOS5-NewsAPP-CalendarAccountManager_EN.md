# HarmonyOS 5 News App Calendar Account Management Implementation

## Abstract
This article introduces the implementation of the calendar account management function in the HarmonyOS 5.0 news application. It includes buttons for adding specific types of calendar accounts and a builder for deleting calendar accounts. The ArkTS language is used to implement the account addition and deletion operations.

## Implementation Steps
1. Implement the account addition button component.
2. Implement the account deletion builder.
3. Bind the logic for account addition and deletion operations.

## Code Implementation
### 1. Implement the Account Addition Button Component
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

### 2. Implement the Account Deletion Builder
```typescript
@Builder 
DeleteBuilder (calendar: calendarManager.Calendar) { 
  Text('Delete') 
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

### 3. Operation Logic Explanation
When clicking the account addition button, the `addAccount` method is called to create the corresponding type of calendar account, and then the account list is updated. When clicking the delete button, the `deleteCalendar` method is called to delete the specified account, and the account list is also updated.

## Other API Explanations

getCalendarManager(context: Context): CalendarManager	Get the `CalendarManager` object based on the context to manage calendars.

createCalendar(calendarAccount: CalendarAccount): Promise<Calendar>	Create a `Calendar` object based on the calendar account information using a Promise for asynchronous callbacks.

getCalendar(calendarAccount?: CalendarAccount): Promise<Calendar>
Get the default `Calendar` object or a specified `Calendar` object using a Promise for asynchronous callbacks.

The default `Calendar` is created when the calendar storage runs for the first time. If you don't care about the `Calendar` attribution when creating an `Event`, you don't need to create a `Calendar` through `createCalendar()`. You can directly use the default `Calendar`.

getAllCalendars(): Promise<Calendar[]>	Get all `Calendar` objects created by the current application and the default `Calendar` object using a Promise for asynchronous callbacks.

deleteCalendar(calendar: Calendar): Promise<void>	Delete the specified `Calendar` object using a Promise for asynchronous callbacks.

getConfig(): CalendarConfig	Get the calendar configuration information.

setConfig(config: CalendarConfig): Promise<void>	Set the calendar configuration information using a Promise for asynchronous callbacks.

getAccount(): CalendarAccount	Get the calendar account information.

## Summary
Key knowledge points include the use of the `@Builder` decorator, button click event handling, asynchronous function calls, and state updates. Through these implementations, the functions of adding and deleting calendar accounts are achieved, and the account list display is updated after each operation.

## Application Scenarios
Calendar Kit provides calendar and schedule management capabilities, which generally refers to APIs (Application Programming Interfaces) that can be used to access and manipulate calendar data. These interfaces allow developers to integrate time - related schedule services from other applications (such as travel, dining, sports, and entertainment) with the system calendar, enabling functions such as schedule management, event creation, and query.

One - click Schedule Service: Through a permanent authorization mechanism, after the user permits the current application to read and write the system calendar, developers can write the corresponding schedule services into the calendar in the form of deeplink. According to the reminder rules set by the developer, when the schedule is approaching or due, the corresponding "service buttons" will be displayed in the calendar application, calendar notifications, calendar cards, etc. Users can click these buttons to launch the jump link and directly reach the service landing page in one step.

## Constraints and Limitations
Currently, the relevant capabilities and interfaces of Calendar Kit can only be used in the Stage model.

To use the relevant capabilities of Calendar Kit, you need to obtain the permissions to read or write calendars and schedules. For specific guidance, please refer to the permission declaration.

When reading calendars or schedules, you need to apply for the `ohos.permission.READ_CALENDAR` permission.

When adding, deleting, or modifying calendars or schedules, you need to apply for the `ohos.permission.WRITE_CALENDAR` permission.