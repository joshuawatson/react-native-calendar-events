# React-Native-Calendar-Events
React Native Module for IOS Calendar Events

## Install
```
npm install react-native-calendar-events
```
Then add `RNCalendarEvents`, as well as `EventKit.framework` to project libraries.

For iOS 8 compatibility, you may need to link your project with `CoreFoundation.framework` (status = Optional) under Link Binary With Libraries on the Build Phases page of your project settings.

## Usage

Require the `react-native-calendar-events` module.

```javascript
import RNCalendarEvents from 'react-native-calendar-events';
```

## Properties

| Property        | Value            | Description |
| :--------------- | :---------------- | :----------- |
| id              | String (read-only)             | Unique id for the calendar event. |
| title           | String             | The title for the calendar event. |
| startDate       | Date             | The start date of the calendar event. |
| endDate         | Date             | The end date of the calendar event. |
| allDay          | Bool             | Indicates whether the event is an all-day event. |
| recurrence      | String           | The simple recurrence frequency of the calendar event `daily`, `weekly`, `monthly`, `yearly` or none. |
| occurrenceDate  | Date (read-only) | The original occurrence date of an event if it is part of a recurring series. |
| isDetached      | Bool             | Indicates whether an event is a detached instance of a repeating event. |
| location        | String           | The location associated with the calendar event. |
| notes           | String           | The notes associated with the calendar event. |
| alarms          | Array            | The alarms associated with the calendar event, as an array of alarm objects. |


## Get authorization status for IOS EventStore
```
authorizationStatus()
```
**Parameters:** *None*

**Returns:** *Promise*

**onFulfill:** 
 - Fulfillment value: `String` : Either 'denied', 'restricted', 'authorized' or 'undetermined'

Example:
```javascript
RNCalendarEvents.authorizationStatus()
.then(status => {
  // handle status
})
.catch(error => {
  // handle error
});
```

## Request authorization to IOS EventStore
Authorization must be granted before accessing calendar events.

```javascript
authorizeEventStore();
```

**Parameters:** *None*

**Returns:** *Promise*

**onFulfill:** 
  - Fulfillment value: `String` : Either 'denied', 'restricted', 'authorized' or 'undetermined'

Example
```javascript
RNCalendarEvents.authorizeEventStore()
.then(status => {
  // handle status
})
.catch(error => {
  // handle error
});
```

## Fetch all calendar events from EventStore

```javascript
fetchAllEvents(startDate, endDate);
```

**Parameters:** <br>
 - startDate: `Date`
 - endDate: `Date`

**Returns:** *Promise*

**onFulfill:** 
 - Fulfillment value: `Array` : Event found based on the predicate's start and end date.

Example
```javascript
RNCalendarEvents.fetchAllEvents('2016-08-19T19:26:00.000Z', '2017-08-19T19:26:00.000Z')
.then(events => {
  // handle events
})
.catch(error => {
  // handle error
});
```

## Create calendar event

```
saveEvent(title, settings);
```

**Parameters:** <br>
 - title: `String` : Title of the event
 - settings: `Object` : Settings for the event

**Returns:** *Promise*

**onFulfill:**
 - Fulfillment value: `String` : Newly created event's id.

Example:

```javascript
RNCalendarEvents.saveEvent('title', {
  location: 'location',
  notes: 'notes',
  startDate: '2016-10-01T09:45:00.000UTC',
  endDate: '2016-10-02T09:45:00.000UTC'
})
.then(id => {
  // handle new event 
})
.catch(error => {
  // handle error
});
```

## Create calendar event with alarms

### Alarm options:

| Property        | Value            | Description |
| :--------------- | :------------------| :----------- |
| date           | Date or Number    | If a Date is given, an alarm will be set with an absolute date. If a Number is given, an alarm will be set with a relative offset (in minutes) from the start date. |
| structuredLocation | Object             | The location to trigger an alarm. |

### Alarm structuredLocation properties:

| Property        | Value            | Description |
| :--------------- | :------------------| :----------- |
| title           | String  | The title of the location.|
| proximity | String             | A value indicating how a location-based alarm is triggered. Possible values: `enter`, `leave`, `none`. |
| radius | Number             | A minimum distance from the core location that would trigger the calendar event's alarm. |
| coords | Object             | The geolocation coordinates, as an object with latitude and longitude properties |

Example with date:

```javascript
RNCalendarEvents.saveEvent('title', {
  location: 'location',
  notes: 'notes',
  startDate: '2016-10-01T09:45:00.000UTC',
  endDate: '2016-10-02T09:45:00.000UTC',
  alarms: [{
    date: -1 // or absolute date
  }]
});

```
Example with structuredLocation:

```javascript
RNCalendarEvents.saveEvent('title', {
  location: 'location',
  notes: 'notes',
  startDate: '2016-10-01T09:45:00.000UTC',
  endDate: '2016-10-02T09:45:00.000UTC',
  alarms: [{
    structuredLocation: {
      title: 'title',
      proximity: 'enter',
      radius: 500,
      coords: {
        latitude: 30.0000,
        longitude: 97.0000
      }
    }
  }]
});
```

Example with recurrence:

```javascript
RNCalendarEvents.saveEvent('title', {
  location: 'location',
  notes: 'notes',
  startDate: '2016-10-01T09:45:00.000UTC',
  endDate: '2016-10-02T09:45:00.000UTC',
  alarms: [{
    date: -1 // or absolute date
  }],
  recurrence: 'daily'
});
```

## Update calendar event
Add `id` property to the settings object to update an existing calendar event.

**Parameters:** <br>
 - title: `String` : Title of the event
 - settings: `Object` : Settings for the event

**Returns:** *Promise*

**onFulfill:**
 - Fulfillment value: `String` : Updated event's id.

Example:
```javascript
RNCalendarEvents.saveEvent('title', {
  id: 'id',
  location: 'location',
  notes: 'notes',
  startDate: '2016-10-02T09:45:00.000UTC',
  endDate: '2016-10-02T09:45:00.000UTC'
});
```

## Remove calendar event
Give the unique calendar event instance **id** to remove the calendar event.

```javascript
removeEvent('id');
```

**Parameters:** <br>
 - id: `String` : Unique id for the event.

**Returns:** *Promise*

**onFulfill:**
 - Fulfillment value: `String` : Removed event id.

Example:
```javascript
RNCalendarEvents.removeEvent('id')
.then(id => {
  // handle removed event
})
.catch(error => {
  // handle error
});
```

## Remove future (recurring) calendar events
Give the unique calendar event instance **id** to remove future calendar events.

**Parameters:** <br>
 - id: `String` : Unique id for the event.

**Returns:** *Promise*

**onFulfill:**
 - Fulfillment value: `String` : Removed event id.


```javascript
RNCalendarEvents.removeFutureEvents('id')
.then(id => {
  // handle removed event
})
.catch(error => {
  // handle error
});
```
