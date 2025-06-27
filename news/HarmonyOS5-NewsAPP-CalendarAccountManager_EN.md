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

## Summary
Key knowledge points include the use of the `@Builder` decorator, button click event handling, asynchronous function calls, and state updates. Through these implementations, the functions of adding and deleting calendar accounts are achieved, and the account list display is updated after each operation.