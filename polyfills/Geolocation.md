/!\ ATTENTION /!\ You need to add **NSLocationWhenInUseUsageDescription** key in Info.plist to enable geolocation, otherwise it's going to *fail silently*! !/ !/

Geolocation follows the MDN specification: [https://developer.mozilla.org/en-US/docs/Web/API/Geolocation](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation)

## Methods 

static **getCurrentPosition**(geo_success: Function, geo_error?: Function, geo_options?: Object)

static **watchPosition**(success: Function, error?: Function, options?: Object) 

static **clearWatch**(watchID: number) 

static **stopObserving**() 

## Examples 

```javascript
/* eslint no-console: 0 */
'use strict';


var React = require('react-native');
var {
  StyleSheet,
  Text,
  View,
} = React;

exports.framework = 'React';
exports.title = 'Geolocation';
exports.description = 'Examples of using the Geolocation API.';

exports.examples = [
  {
    title: 'navigator.geolocation',
    render: function(): ReactElement {
      return <GeolocationExample />;
    },
  }
];

var GeolocationExample = React.createClass({
  watchID: (null: ?number),

  getInitialState: function() {
    return {
      initialPosition: 'unknown',
      lastPosition: 'unknown',
    };
  },

  componentDidMount: function() {
    navigator.geolocation.getCurrentPosition(
      (initialPosition) => this.setState({initialPosition}),
      (error) => console.error(error)
    );
    this.watchID = navigator.geolocation.watchPosition((lastPosition) => {
      this.setState({lastPosition});
    });
  },

  componentWillUnmount: function() {
    navigator.geolocation.clearWatch(this.watchID);
  },

  render: function() {
    return (
      <View>
        <Text>
          <Text style={styles.title}>Initial position: </Text>
          {JSON.stringify(this.state.initialPosition)}
        </Text>
        <Text>
          <Text style={styles.title}>Current position: </Text>
          {JSON.stringify(this.state.lastPosition)}
        </Text>
      </View>
    );
  }
});

var styles = StyleSheet.create({
  title: {
    fontWeight: '500',
  },
});
```