`AppStateIOS` can tell you if the app is in the foreground or background, and notify you when the state changes.

`AppStateIOS` is frequently used to determine the intent and proper behavior when handling push notifications.

## iOS App States 

* `active` - The app is running in the foreground
* `background` - The app is running in the background. The user is either in another app or on the home screen
* `inactive` - This is a transition state that currently never happens for typical React Native apps.

For more information, see [Apple's documentation](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/TheAppLifeCycle/TheAppLifeCycle.html)

## Basic Usage 

To see the current state, you can check AppStateIOS.currentState, which will be kept up-to-date. However, currentState will be null at launch while AppStateIOS retrieves it over the bridge.

```javascript
getInitialState: function() {
  return {
    currentAppState: AppStateIOS.currentState,
  };
},
componentDidMount: function() {
  AppStateIOS.addEventListener('change', this._handleAppStateChange);
},
componentWillUnmount: function() {
  AppStateIOS.removeEventListener('change', this._handleAppStateChange);
},
_handleAppStateChange: function(currentAppState) {
  this.setState({ currentAppState, });
},
render: function() {
  return (
    <Text>Current state is: {this.state.currentAppState}</Text>
  );
},
```

This example will only ever appear to say "Current state is: active" because the app is only visible to the user when in the `active` state, and the null state will happen only momentarily.

## Methods 

static **addEventListener**(type: string, handler: Function) 

Add a handler to AppState changes by listening to the change event type and providing the handler

static **removeEventListener**(type: string, handler: Function) 

Remove a handler by passing the change event type and the handler

## Examples 

```javascript
'use strict';

var React = require('react-native');
var {
  AppStateIOS,
  Text,
  View
} = React;

var AppStateSubscription = React.createClass({
  getInitialState() {
    return {
      appState: AppStateIOS.currentState,
      previousAppStates: [],
    };
  },
  componentDidMount: function() {
    AppStateIOS.addEventListener('change', this._handleAppStateChange);
  },
  componentWillUnmount: function() {
    AppStateIOS.removeEventListener('change', this._handleAppStateChange);
  },
  _handleAppStateChange: function(appState) {
    var previousAppStates = this.state.previousAppStates.slice();
    previousAppStates.push(this.state.appState);
    this.setState({
      appState,
      previousAppStates,
    });
  },
  render() {
    if (this.props.showCurrentOnly) {
      return (
        <View>
          <Text>{this.state.appState}</Text>
        </View>
      );
    }
    return (
      <View>
        <Text>{JSON.stringify(this.state.previousAppStates)}</Text>
      </View>
    );
  }
});

exports.title = 'AppStateIOS';
exports.description = 'iOS app background status';
exports.examples = [
  {
    title: 'AppStateIOS.currentState',
    description: 'Can be null on app initialization',
    render() { return <Text>{AppStateIOS.currentState}</Text>; }
  },
  {
    title: 'Subscribed AppStateIOS:',
    description: 'This changes according to the current state, so you can only ever see it rendered as "active"',
    render(): ReactElement { return <AppStateSubscription showCurrentOnly={true} />; }
  },
  {
    title: 'Previous states:',
    render(): ReactElement { return <AppStateSubscription showCurrentOnly={false} />; }
  },
];
```