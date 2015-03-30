A wrapper for making views respond properly to touches. On press down, the opacity of the wrapped view is decreased, which allows the underlay color to show through, darkening or tinting the view. The underlay comes from adding a view to the view hierarchy, which can sometimes cause unwanted visual artifacts if not used correctly, for example if the backgroundColor of the wrapped view isn't explicitly set to an opaque color.

Example:

```html
renderButton: function() {
  return (
    <TouchableHighlight onPress={this._onPressButton}>
      <Image
        style={styles.button}
        source={require('image!myButton')}
      />
    </View>
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