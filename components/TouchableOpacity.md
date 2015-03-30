A wrapper for making views respond properly to touches. On press down, the opacity of the wrapped view is decreased, dimming it. This is done without actually changing the view hierarchy, and in general is easy to add to an app without weird side-effects.

Example:

```html
renderButton: function() {
  return (
    <TouchableOpacity onPress={this._onPressButton}>
      <Image
        style={styles.button}
        source={require('image!myButton')}
      />
    </View>
  );
},
```

## Props 

[TouchableWithoutFeedback props...](http://facebook.github.io/react-native/docs/touchablewithoutfeedback.html#proptypes)

**activeOpacity** number 

Determines what the opacity of the wrapped view should be when touch is active.