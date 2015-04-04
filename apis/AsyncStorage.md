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