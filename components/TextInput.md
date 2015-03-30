A foundational component for inputting text into the app via a keyboard. Props provide configurability for several features, such as auto- correction, auto-capitalization, placeholder text, and different keyboard types, such as a numeric keypad.

The simplest use case is to plop down a `TextInput` and subscribe to the `onChangeText` events to read the user input. There are also other events, such as `onSubmitEditing` and `onFocus` that can be subscribed to. A simple example:

```html
<View>
  <TextInput
    style={{height: 40, borderColor: 'gray', borderWidth: 1}}
    onChangeText={(text) => this.setState({input: text})}
  />
  <Text>{'user input: ' + this.state.input}</Text>
</View>
```

The `value` prop can be used to set the value of the input in order to make the state of the component clear, but <TextInput> does not behave as a true controlled component by default because all operations are asynchronous. Setting `value` once is like setting the default value, but you can change it continuously based on `onChangeText` events as well. If you really want to force the component to always revert to the value you are setting, you can set `controlled={true}`.

The `multiline` prop is not supported in all releases, and some props are multiline only.

## Props 

**autoCapitalize** enum('none', 'sentences', 'words', 'characters') 

Can tell TextInput to automatically capitalize certain characters.

* characters: all characters,
* words: first letter of each word
* sentences: first letter of each sentence (default)
* none: don't auto capitalize anything

**autoCorrect** bool 

If false, disables auto-correct. Default value is true.

**autoFocus** bool 

If true, focuses the input on componentDidMount. Default value is false.

**bufferDelay** number 

This helps avoid drops characters due to race conditions between JS and the native text input. The default should be fine, but if you're potentially doing very slow operations on every keystroke then you may want to try increasing this.

**clearButtonMode** enum('never', 'while-editing', 'unless-editing', 'always') 

When the clear button should appear on the right side of the text view

**controlled** bool 

If you really want this to behave as a controlled component, you can set this true, but you will probably see flickering, dropped keystrokes, and/or laggy typing, depending on how you process onChange events.

**editable** bool 

If false, text is not editable. Default value is true.

**keyboardType** enum('default', 'numeric') 

Determines which keyboard to open, e.g.`numeric`.

**multiline** bool 

If true, the text input can be multiple lines. Default value is false.

**onBlur** function 

Callback that is called when the text input is blurred

**onChangeText** function 

(text: string) => void

Callback that is called when the text input's text changes.

**onEndEditing** function 

**onFocus** function 

Callback that is called when the text input is focused

**onSubmitEditing** function 

**placeholder** string 

The string that will be rendered before text input has been entered

**placeholderTextColor** string 

The text color of the placeholder string

**selectionState** DocumentSelectionState 

See DocumentSelectionState.js, some state that is responsible for maintaining selection information for a document

**style** [Text#style](http://facebook.github.io/react-native/docs/text.html#style)

**value** string 

The default value for the text input