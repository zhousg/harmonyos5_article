# HarmonyOS 5 News App Calendar Event Management Implementation

## Abstract
This article introduces the implementation of the calendar event management function in the HarmonyOS 5.0 news application, including binding pop - up windows, adding calendar events, and displaying event details. It is implemented using the ArkTS language.

## Implementation Steps
1. Bind the pop - up component.
2. Implement the function to add calendar events.
3. Build the account details component to display the event list.

## Code Implementation
### 1. Bind the Pop - up Component
```typescript
.bindSheet($$this.isShow, this.AccountDetailBuilder, { 
  height: 500 
}) 
```

### 2. Implement the Function to Add Calendar Events
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

### 3. Build the Account Details Component to Display the Event List
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

## Summary
Key knowledge points include pop - up binding, asynchronous function calls, event addition, and list display. The `addCalendarEvent` method is used to add events, the `AccountDetailBuilder` component is used to display subscribed news events, and the `List` and `ForEach` components are used to display the event list.

## Other API Explanations

getCalendarManager(context: Context): CalendarManager	Get the CalendarManager object based on the context to manage the calendar.

createCalendar(calendarAccount: CalendarAccount): Promise<Calendar>	Create a Calendar object based on the calendar account information using a Promise asynchronous callback.

addEvent(event: Event): Promise<number>	Create an event. The input parameter Event does not need to include the event ID. Use a Promise asynchronous callback.

editEvent(event: Event): Promise<number>	Create a single event. The input parameter Event does not need to include the event ID. Calling this interface will redirect to the event creation page. Use a Promise asynchronous callback.

deleteEvent(id: number): Promise<void>	Delete the event with the specified event ID using a Promise asynchronous callback.

updateEvent(event: Event): Promise<void>	Update an event using a Promise asynchronous callback.

getEvents(eventFilter?: EventFilter, eventKey?: (keyof Event)[]): Promise<Event[]>	Get the events under the Calendar that match the query conditions using a Promise asynchronous callback.

## Notes
A schedule refers to a specific event or activity arrangement. Schedule management involves planning and controlling these events and activities, which can help make more effective use of relevant resources, improve productivity and efficiency, and enable people to better manage their time and tasks.

In Calendar Kit, a schedule (Event) belongs to a corresponding calendar account (Calendar). One calendar account can have multiple schedules, and one schedule only belongs to one Calendar.

After obtaining the calendar account object, you can manage the schedules under this account, including creating, deleting, modifying, and querying schedules. When creating or modifying a schedule, you can set relevant information such as the schedule's title, start time, end time, schedule type, schedule location, schedule reminder time, and schedule recurrence rules to achieve more comprehensive and effective schedule management.