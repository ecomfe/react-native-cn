Handle push notifications for your app, including permission handling and icon badge number.

To get up and running, [configure your notifications with Apple](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/ConfiguringPushNotifications/ConfiguringPushNotifications.html) and your server-side system. To get an idea, [this is the Parse guide](https://parse.com/tutorials/ios-push-notifications).

## Methods 

static **setApplicationIconBadgeNumber**(number: number) 

Sets the badge number for the app icon on the home screen

static **getApplicationIconBadgeNumber**(callback: Function) 

Gets the current badge number for the app icon on the home screen

static **addEventListener**(type: string, handler: Function) 

Attaches a listener to remote notifications while the app is running in the foreground or the background.

The handler will get be invoked with an instance of `PushNotificationIOS`

static **requestPermissions**() 

Requests all notification permissions from iOS, prompting the user's dialog box.

static **checkPermissions**(callback: Function) 

See what push permissions are currently enabled. callback will be invoked with a `permissions` object:

* `alert` :boolean
* `badge` :boolean
* `sound` :boolean

static **removeEventListener**(type: string, handler: Function) 

Removes the event listener. Do this in componentWillUnmount to prevent memory leaks

static **popInitialNotification**() 

An initial notification will be available if the app was cold-launched from a notification.

The first caller of `popInitialNotification` will get the initial notification object, or null. Subsequent invocations will return null.

**constructor**(nativeNotif) 

You will never need to instansiate `PushNotificationIOS` yourself. Listening to the `notification` event and invoking `popInitialNotification` is sufficient

**getMessage**() 

An alias for `getAlert` to get the notification's main message string

**getSound**() 

Gets the sound string from the `aps` object

**getAlert**() 

Gets the notification's main message from the `aps` object

**getBadgeCount**() 

Gets the badge count number from the `aps` object

**getData**() 

Gets the data object on the notif

## Examples 

```javascript
'use strict';

var React = require('react-native');
var {
  AlertIOS,
  PushNotificationIOS,
  StyleSheet,
  Text,
  TouchableHighlight,
  View,
} = React;

var Button = React.createClass({
  render: function() {
    return (
      <TouchableHighlight
        underlayColor={'white'}
        style={styles.button}
        onPress={this.props.onPress}>
        <Text style={styles.buttonLabel}>
          {this.props.label}
        </Text>
      </TouchableHighlight>
    );
  }
});

class NotificationExample extends React.Component {
  componentWillMount() {
    PushNotificationIOS.addEventListener('notification', this._onNotification);
  }

  componentWillUnmount() {
    PushNotificationIOS.removeEventListener('notification', this._onNotification);
  }

  render() {
    return (
      <View>
        <Button
          onPress={this._sendNotification}
          label="Send fake notification"
        />
      </View>
    );
  }

  _sendNotification() {
    require('RCTDeviceEventEmitter').emit('remoteNotificationReceived', {
      aps: {
        alert: 'Sample notification',
        badge: '+1',
        sound: 'default',
        category: 'REACT_NATIVE'
      },
    });
  }

  _onNotification(notification) {
    AlertIOS.alert(
      'Notification Received',
      'Alert message: ' + notification.getMessage(),
      [{
        text: 'Dismiss',
        onPress: null,
      }]
    );
  }
}

class NotificationPermissionExample extends React.Component {
  constructor(props) {
    super(props);
    this.state = {permissions: null};
  }

  render() {
    return (
      <View>
        <Button
          onPress={this._showPermissions.bind(this)}
          label="Show enabled permissions"
        />
        <Text>
          {JSON.stringify(this.state.permissions)}
        </Text>
      </View>
    );
  }

  _showPermissions() {
    PushNotificationIOS.checkPermissions((permissions) => {
      this.setState({permissions});
    });
  }
}

var styles = StyleSheet.create({
  button: {
    padding: 10,
    alignItems: 'center',
    justifyContent: 'center',
  },
  buttonLabel: {
    color: 'blue',
  },
});

exports.title = 'PushNotificationIOS';
exports.description = 'Apple PushNotification and badge value';
exports.examples = [
{
  title: 'Badge Number',
  render(): React.Component {
    PushNotificationIOS.requestPermissions();

    return (
      <View>
        <Button
          onPress={() => PushNotificationIOS.setApplicationIconBadgeNumber(42)}
          label="Set app's icon badge to 42"
        />
        <Button
          onPress={() => PushNotificationIOS.setApplicationIconBadgeNumber(0)}
          label="Clear app's icon badge"
        />
      </View>
    );
  },
},
{
  title: 'Push Notifications',
  render(): React.Component {
    return <NotificationExample />;
  }
},
{
  title: 'Notifications Permissions',
  render(): React.Component {
    return <NotificationPermissionExample />;
  }
}];
```