A wrapper for making views respond properly to touches. On press down, the opacity of the wrapped view is decreased, which allows the underlay color to show through, darkening or tinting the view. The underlay comes from adding a view to the view hierarchy, which can sometimes cause unwanted visual artifacts if not used correctly, for example if the backgroundColor of the wrapped view isn't explicitly set to an opaque color.

Example:

```html
renderButton: function () {
    return (
        <TouchableHighlight onPress={this._onPressButton}>
            <Image
                style={styles.button}
                source={require('image!myButton')}
            />
        </TouchableHighlight>
    );
},
```

## Props 

[**TouchableWithoutFeedback props...**](http://facebook.github.io/react-native/docs/touchablewithoutfeedback.html#proptypes)

**activeOpacity** number 

Determines what the opacity of the wrapped view should be when touch is active.

**style** [View#style](http://facebook.github.io/react-native/docs/view.html#style)

**underlayColor** string 

The color of the underlay that will show through when the touch is active.


一个能让视图响应触摸事件的封装器。当点击封装了的视图时，它的透明度会减小，使得视图的颜色变暗变浅，同时让衬底颜色透过它显示出来。当我们通过往该视图的下层添加新视图的方法来添加衬底时，如果使用不当，比如当这个上层被封装的视图的背景颜色没有明确地设置为不透明色时，就会显示出正常情况下并不想看见的内容。

例子:

```html
renderButton: function () {
    return (
        <TouchableHighlight onPress={this._onPressButton}>
            <Image
                style={styles.button}
                source={require('image!myButton')}
            />
        </TouchableHighlight>
    );
},
```

## 属性

[**TouchableWithoutFeedback 属性...**](https://github.com/ecomfe/react-native-cn/blob/master/components/TouchableWithoutFeedback.md)

**activeOpacity** number

决定了封装的视图被触摸时的透明度。

**style** [View#style](https://github.com/ecomfe/react-native-cn/blob/working/components/View.md#style)

**underlayColor** string

当触摸时会被显示出来的衬底颜色
