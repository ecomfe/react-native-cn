Use `SwitchIOS` to render a boolean input on iOS. This is a controlled component, so you must hook in to the `onValueChange` callback and update the `value` prop in order for the component to update, otherwise the user's change will be reverted immediately to reflect `props.value` as the source of truth.

## Props 

**disabled** bool 

If true the user won't be able to toggle the switch. Default value is false.

**onTintColor** string 

Background color when the switch is turned on.

**onValueChange** function 

Callback that is called when the user toggles the switch.

**thumbTintColor** string 

Background color for the switch round button.

**tintColor** string 

Background color when the switch is turned off.

**value** bool 

The value of the switch, if true the switch will be turned on. Default value is false.