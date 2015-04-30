All rights reserved.

This source code is licensed under the BSD-style license found in the LICENSE file in the root directory of this source tree. An additional grant of patent rights can be found in the PATENTS file in the same directory.

@flow

## Methods 

static **setStyle**(style: number, animated?: boolean) 

static **setHidden**(hidden: boolean, animation: number) 

## Examples 

```javascript
'use strict';

var React = require('react-native');
var {
  StyleSheet,
  View,
  Text,
  TouchableHighlight,
  StatusBarIOS,
} = React;

exports.framework = 'React';
exports.title = 'StatusBarIOS';
exports.description = 'Module for controlling iOS status bar';
exports.examples = [{
  title: 'Status Bar Style',
  render() {
    return (
      <View>
        {Object.keys(StatusBarIOS.Style).map((key) =>
          <TouchableHighlight style={styles.wrapper}
            onPress={() => StatusBarIOS.setStyle(StatusBarIOS.Style[key])}>
            <View style={styles.button}>
              <Text>setStyle(StatusBarIOS.Style.{key})</Text>
            </View>
          </TouchableHighlight>
        )}
      </View>
    );
  },
}, {
  title: 'Status Bar Style Animated',
  render() {
    return (
      <View>
        {Object.keys(StatusBarIOS.Style).map((key) =>
          <TouchableHighlight style={styles.wrapper}
            onPress={() => StatusBarIOS.setStyle(StatusBarIOS.Style[key], true)}>
            <View style={styles.button}>
              <Text>setStyle(StatusBarIOS.Style.{key}, true)</Text>
            </View>
          </TouchableHighlight>
        )}
      </View>
    );
  },
}, {
  title: 'Status Bar Hidden',
  render() {
    return (
      <View>
        {Object.keys(StatusBarIOS.Animation).map((key) =>
          <View>
            <TouchableHighlight style={styles.wrapper}
              onPress={() => StatusBarIOS.setHidden(true, StatusBarIOS.Animation[key])}>
              <View style={styles.button}>
                <Text>setHidden(true, StatusBarIOS.Animation.{key})</Text>
              </View>
            </TouchableHighlight>
            <TouchableHighlight style={styles.wrapper}
              onPress={() => StatusBarIOS.setHidden(false, StatusBarIOS.Animation[key])}>
              <View style={styles.button}>
                <Text>setHidden(false, StatusBarIOS.Animation.{key})</Text>
              </View>
            </TouchableHighlight>
          </View>
        )}
      </View>
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

版权所有

This source code is licensed under the BSD-style license found in the LICENSE file in the root directory of this source tree. An additional grant of patent rights can be found in the PATENTS file in the same directory.

这里的所有代码都遵循源码树的根目录下的LICENSE文件中的BSD协议。追加的专利授权可以在相同目录的PATENTS文件中找到。

@flow

## Methods 

static **setStyle**(style: number, animated?: boolean) 

设置IOS状态栏的样式。

static **setHidden**(hidden: boolean, animation: number)

设置IOS状态栏的西安隐状态。
