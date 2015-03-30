Use `DatePickerIOS` to render a date/time picker (selector) on iOS. This is a controlled component, so you must hook in to the `onDateChange` callback and update the `date` prop in order for the component to update, otherwise the user's change will be reverted immediately to reflect `props.date` as the source of truth.

##Props 
**date** Date 

The currently selected date.

**maximumDate** Date 

Maximum date.

Restricts the range of possible date/time values.

**minimumDate** Date 

Minimum date.

Restricts the range of possible date/time values.

**minuteInterval** enum(1, 2, 3, 4, 5, 6, 10, 12, 15, 20, 30) 

The interval at which minutes can be selected.

mode enum('date', 'time', 'datetime') 

The date picker mode.

**onDateChange** function 

Date change handler.

This is called when the user changes the date or time in the UI. The first and only argument is a Date object representing the new date and time.

**timeZoneOffsetInMinutes** number 

Timezone offset in minutes.

By default, the date picker will use the device's timezone. With this parameter, it is possible to force a certain timezone offset. For instance, to show times in Pacific Standard Time, pass -7 * 60.