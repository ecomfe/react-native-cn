The Vibration API is exposed at VibrationIOS.vibrate(). On iOS, calling this function will trigger a one second vibration. The vibration is asynchronous so this method will return immediately.

There will be no effect on devices that do not support Vibration, eg. the iOS simulator.

Vibration patterns are currently unsupported.

## Methods 

static **vibrate**() 

## Examples 

```javascript
'use strict';

var React = require('react-native');
var {
  StyleSheet,
  View,
  Text,
  TouchableHighlight,
  VibrationIOS
} = React;

exports.framework = 'React';
exports.title = 'VibrationIOS';
exports.description = 'Vibration API for iOS';
exports.examples = [{
  title: 'VibrationIOS.vibrate()',
  render() {
    return (
      <TouchableHighlight
        style={styles.wrapper}
        onPress={() => VibrationIOS.vibrate()}>
        <View style={styles.button}>
          <Text>Vibrate</Text>
        </View>
      </TouchableHighlight>
    );
  },
}];

var styles = StyleSheet.create({
  wrapper: {
    borderRadius: 5,
    marginBottom: 5,
  },
  button: {
    backgroundColor: '#eeeeee',
    padding: 10,
  },
});
```