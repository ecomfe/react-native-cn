AsyncStorage is a simple, asynchronous, persistent, global, key-value storage system. It should be used instead of LocalStorage.

It is recommended that you use an abstraction on top of AsyncStorage instead of AsyncStorage directly for anything more than light usage since it operates globally.

This JS code is a simple facad over the native iOS implementation to provide a clear JS API, real Error objects, and simple non-multi functions.

## Methods 

static **getItem**(key: string, callback: (error: ?Error, result: ?string) => void) 

Fetches `key` and passes the result to `callback`, along with an `Error` if there is any.

static **setItem**(key: string, value: string, callback: ?(error: ?Error) => void) 

Sets `value` for `key` and calls `callback` on completion, along with an `Error` if there is any.

static **removeItem**(key: string, callback: ?(error: ?Error) => void) 

static **mergeItem**(key: string, value: string, callback: ?(error: ?Error) => void) 

Merges existing value with input value, assuming they are stringified json.

Not supported by all native implementations.

static **clear**(callback: ?(error: ?Error) => void) 

Erases *all* AsyncStorage for all clients, libraries, etc. You probably don't want to call this - use removeItem or multiRemove to clear only your own keys instead.

static **getAllKeys**(callback: (error: ?Error) => void) 

Gets *all* keys known to the system, for all callers, libraries, etc.

static **multiGet**(keys: Array<string>, callback: (errors: ?Array<Error>, result: ?Array<Array<string>>) => void) 

multiGet invokes callback with an array of key-value pair arrays that matches the input format of multiSet.

multiGet(['k1', 'k2'], cb) -> cb([['k1', 'val1'], ['k2', 'val2']])

static **multiSet**(keyValuePairs: Array<Array<string>>, callback: ?(errors: ?Array<Error>) => void) 

multiSet and multiMerge take arrays of key-value array pairs that match the output of multiGet, e.g.

multiSet([['k1', 'val1'], ['k2', 'val2']], cb);

static **multiRemove**(keys: Array<string>, callback: ?(errors: ?Array<Error>) => void) 

Delete all the keys in the `keys` array.

static **multiMerge**(keyValuePairs: Array<Array<string>>, callback: ?(errors: ?Array<Error>) => void) 

Merges existing values with input values, assuming they are stringified json.

Not supported by all native implementations.

## Examples 

```javascript
'use strict';

var React = require('react-native');
var {
  AsyncStorage,
  PickerIOS,
  Text,
  View
} = React;
var PickerItemIOS = PickerIOS.Item;

var STORAGE_KEY = '@AsyncStorageExample:key';
var COLORS = ['red', 'orange', 'yellow', 'green', 'blue'];

var BasicStorageExample = React.createClass({
  componentDidMount() {
    AsyncStorage.getItem(STORAGE_KEY, (error, value) => {
      if (error) {
        this._appendMessage('AsyncStorage error: ' + error.message);
      } else if (value !== null) {
        this.setState({selectedValue: value});
        this._appendMessage('Recovered selection from disk: ' + value);
      } else {
        this._appendMessage('Initialized with no selection on disk.');
      }
    });
  },
  getInitialState() {
    return {
      selectedValue: COLORS[0],
      messages: [],
    };
  },

  render() {
    var color = this.state.selectedValue;
    return (
      <View>
        <PickerIOS
          selectedValue={color}
          onValueChange={this._onValueChange}>
          {COLORS.map((value) => (
            <PickerItemIOS
              key={value}
              value={value}
              label={value}
            />
          ))}
        </PickerIOS>
        <Text>
          {'Selected: '}
          <Text style={{color}}>
            {this.state.selectedValue}
          </Text>
        </Text>
        <Text>{' '}</Text>
        <Text onPress={this._removeStorage}>
          Press here to remove from storage.
        </Text>
        <Text>{' '}</Text>
        <Text>Messages:</Text>
        {this.state.messages.map((m) => <Text>{m}</Text>)}
      </View>
    );
  },

  _onValueChange(selectedValue) {
    this.setState({selectedValue});
    AsyncStorage.setItem(STORAGE_KEY, selectedValue, (error) => {
      if (error) {
        this._appendMessage('AsyncStorage error: ' + error.message);
      } else {
        this._appendMessage('Saved selection to disk: ' + selectedValue);
      }
    });
  },

  _removeStorage() {
    AsyncStorage.removeItem(STORAGE_KEY, (error) => {
      if (error) {
        this._appendMessage('AsyncStorage error: ' + error.message);
      } else {
        this._appendMessage('Selection removed from disk.');
      }
    });
  },

  _appendMessage(message) {
    this.setState({messages: this.state.messages.concat(message)});
  },
});

exports.title = 'AsyncStorage';
exports.description = 'Asynchronous local disk storage.';
exports.examples = [
  {
    title: 'Basics - getItem, setItem, removeItem',
    render(): ReactElement { return <BasicStorageExample />; }
  },
];
```      
   
异步存储是一个简单的，异步的，持久的和全局性的键值存储系统。它应该取代本地存储而被使用。  
 
因为异步存储是作用于全局的，所以推荐您在异步存储之上添加一个抽象层，而不要仅仅为了轻量级的使用就直接使用异步存储。   
   
为了提供清晰的JS API，真实的错误对象，以及简单的非多元化的功能，这段JS代码在基于本地IOS实现的基础上使用了一个简单的外观模式。  
##方法   
static **getItem**(key: string, callback: (error: ?Error, result: ?string) => void) 
  
获取`key`对应的`value`并将结果传给`回调函数` ,如果有`错误`对象的话，将它和结果一起传给`回调函数`。   


static **setItem**(key: string, value: string, callback: ?(error: ?Error) => void) 

为`key`赋值对应的`value`，在赋值完成后，调用`回调函数`，如果有`错误`对象的话，就将其作为参数传递给`回调函数`。

static **removeItem**(key: string, callback: ?(error: ?Error) => void)
    
static **mergeItem**(key: string, value: string, callback: ?(error: ?Error) => void) 

将key对应的已有的value与参数传进来的value进行合并，前提是这两个值都是字符串化的json对象。

不是所有的本地实现都支持这个方法。

static **clear**(callback: ?(error: ?Error) => void) 

为所有的客户、函数库等清除*所有的*异步存储。如果您只是想清除属于您自己的键值，您可能不想使用这个方法，这种情况下，您可以使用removeItem或者multiRemove方法。

static **getAllKeys**(callback: (error: ?Error) => void) 

为调用者，函数库等获取系统已知的*所有*键值。   

static **multiGet**(keys: Array<string>, callback: (errors: ?Array<Error>, result: ?Array<Array<string>>) => void) 

multiGet操作传进来的参数keys，得到每个key对应的value，将每个key与对应的value组成一个数组，并添加到一个数组中，最后得到一个包含所有键值对数组的数组作为参数传递给回调函数，并唤起回调函数。这个包含键值对数组的数组与multiSet的输入格式是一样的。

比如，multiGet(['k1', 'k2'], cb) -> cb([['k1', 'val1'], ['k2', 'val2']])

static **multiSet**(keyValuePairs: Array<Array<string>>, callback: ?(errors: ?Array<Error>) => void) 

multiSet and multiMerge 的参数与multiGet输出一致，是一个包含很多个键值对数组的数组，比如：
multiSet([['k1', 'val1'], ['k2', 'val2']], cb);

static **multiRemove**(keys: Array<string>, callback: ?(errors: ?Array<Error>) => void) 

删除参数`keys`数组中的所有键值。

static **multiMerge**(keyValuePairs: Array<Array<string>>, callback: ?(errors: ?Array<Error>) => void) 

将key对应的已有的value与参数传进来的value进行合并，前提是这两个值都是字符串化的json对象。

不是所有的本地实现都支持这个方法。

##示例 

```javascript
'use strict';

var React = require('react-native');
var {
  AsyncStorage,
  PickerIOS,
  Text,
  View
} = React;
var PickerItemIOS = PickerIOS.Item;

var STORAGE_KEY = '@AsyncStorageExample:key';
var COLORS = ['red', 'orange', 'yellow', 'green', 'blue'];

var BasicStorageExample = React.createClass({
  componentDidMount() {
    AsyncStorage.getItem(STORAGE_KEY, (error, value) => {
      if (error) {
        this._appendMessage('AsyncStorage error: ' + error.message);
      } else if (value !== null) {
        this.setState({selectedValue: value});
        this._appendMessage('Recovered selection from disk: ' + value);
      } else {
        this._appendMessage('Initialized with no selection on disk.');
      }
    });
  },
  getInitialState() {
    return {
      selectedValue: COLORS[0],
      messages: [],
    };
  },

  render() {
    var color = this.state.selectedValue;
    return (
      <View>
        <PickerIOS
          selectedValue={color}
          onValueChange={this._onValueChange}>
          {COLORS.map((value) => (
            <PickerItemIOS
              key={value}
              value={value}
              label={value}
            />
          ))}
        </PickerIOS>
        <Text>
          {'Selected: '}
          <Text style={{color}}>
            {this.state.selectedValue}
          </Text>
        </Text>
        <Text>{' '}</Text>
        <Text onPress={this._removeStorage}>
          Press here to remove from storage.
        </Text>
        <Text>{' '}</Text>
        <Text>Messages:</Text>
        {this.state.messages.map((m) => <Text>{m}</Text>)}
      </View>
    );
  },

  _onValueChange(selectedValue) {
    this.setState({selectedValue});
    AsyncStorage.setItem(STORAGE_KEY, selectedValue, (error) => {
      if (error) {
        this._appendMessage('AsyncStorage error: ' + error.message);
      } else {
        this._appendMessage('Saved selection to disk: ' + selectedValue);
      }
    });
  },

  _removeStorage() {
    AsyncStorage.removeItem(STORAGE_KEY, (error) => {
      if (error) {
        this._appendMessage('AsyncStorage error: ' + error.message);
      } else {
        this._appendMessage('Selection removed from disk.');
      }
    });
  },

  _appendMessage(message) {
    this.setState({messages: this.state.messages.concat(message)});
  },
});

exports.title = 'AsyncStorage';
exports.description = 'Asynchronous local disk storage.';
exports.examples = [
  {
    title: 'Basics - getItem, setItem, removeItem',
    render(): ReactElement { return <BasicStorageExample />; }
  },
];
```   
