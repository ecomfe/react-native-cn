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

在IOS中用 `DatePickerIOS` 渲染日期/时间选择器. 这是一个受控制的组件, 所以为了组件的更新你必须紧密关联 `onDateChange` 回调并且更新 `date` 属性,不然用户的改变会立即恢复到原状并在  `props.date` 属性上反应出为真.

##属性 
**date** Date 

当前选中的日期.

**maximumDate** Date 

最大日期.

限定日期/时间可能的范围值.

**minimumDate** Date 

最小日期.

限定日期/时间可能的范围值.

**minuteInterval** enum(1, 2, 3, 4, 5, 6, 10, 12, 15, 20, 30) 

可以选中的分钟数的间隔数.

mode enum('date', 'time', 'datetime') 

日期选择器模型.

**onDateChange** function 

日期变化处理函数.

当用户在界面上改变日期或者时间时该函数就会被调用. 第一个也是仅有的参数是一个呈现最新日期时间的日期对象.

**timeZoneOffsetInMinutes** number 

分钟数中的时区偏移量.

默认情况下，日期选择器会使用设备的时区. 有了这个参数, 就有可能强制使用设置的时区偏移量. 比如，想显示太平洋标准时间，传入-7 * 60.
