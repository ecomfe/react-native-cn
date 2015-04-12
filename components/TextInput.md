基本组件在应用程序中通过键盘输入文本。他的属性有几个特点，比如自动校正，自动大写，提示信息文本，以及不同类型的键盘，如数字键盘。最简单的用例就是使用`TextInput`并订阅`onChangeText`事件来读取用户输入。还有其他事件，如`onSubmitEditing`和`onFocus`可以订阅。A simple example:

```html
&lt;View&gt;
  &lt;TextInput
    style={{height: 40, borderColor: 'gray', borderWidth: 1}}
    onChangeText={(text) =&gt; this.setState({input: text})}
  /&gt;
  &lt;Text&gt;{'user input: ' + this.state.input}&lt;/Text&gt;
&lt;/View&gt;
```

`value`属性可以用来设置输入的值，设置`value`是设置为TextInput的默认值，但你可以在`onChangeText`事件中持续的改变他。如果你真的想强制TextInput总是还原到你设置的值，可以设置`controlled= {true}`。`multiline`属性不是在所有版本中支持，有些属性是多行的。## 属性
**autoCapitalize** enum('none', 'sentences', 'words', 'characters') 

他告诉TextInput自动大写某些字符。* characters: 所有字符,
* words: 每一个单词的首字母
* sentences: 每个句子的首字母 (默认)
* none: 不自动大写任何字符

**autoCorrect** bool 

如果设置为false，关闭拼写自动更正功能。 默认值是true。

**autoFocus** bool 

如果设置为true, 在componentDidMount时激活输入。默认值是false。

**bufferDelay** number 

由于JS和原生输入组件存在竞争危害，这个属性有助于避免丢失字符。
默认值应该是没有问题的，但如果你处理每次按键都需要很长时间，那么你可能需要增大这个值。**clearButtonMode** enum('never', 'while-editing', 'unless-editing', 'always') 

何时显示清除按钮在文本视图右侧

**controlled** bool 

如果你真的希望他是可控制的组件，您可以设置为true，但你可能会看到闪烁现象，丢失按键响应，and/or laggy 输入，这些取决于你如何处理onChange事件。**editable** bool 

设置为false，文本是不可编辑的。默认值是true。**keyboardType** enum('default', 'numeric') 

确定要打开的键盘类型，比如数字键盘。**multiline** bool 

如果为true，文本输入可以是多行的。默认值是假的。**onBlur** function 

当输入框失去焦点时调用此函数

**onChangeText** function 

(text: string) =&gt; void

输入的文本更改时被调用

**onEndEditing** function 

**onFocus** function 

当输入框获得焦点时被调用


**onSubmitEditing** function 

**placeholder** string 

能够让你在文本框里显示提示信息，一旦你在文本框里输入了什么信息，提示信息就会隐藏。

**placeholderTextColor** string 

placeholder文字的颜色

**selectionState** DocumentSelectionState 

参考 DocumentSelectionState.js, 这些状态负责维护选中信息

**style** [Text#style](http://facebook.github.io/react-native/docs/text.html#style)

**value** string 

文字框的文字
