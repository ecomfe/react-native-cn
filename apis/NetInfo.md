NetInfo exposes info about online/offline status
## reachabilityIOS 

Asyncronously determine if the device is online and on a cellular network.

* `none` - device is offline
* `wifi` - device is online and connected via wifi, or is the iOS simulator
* `cell` - device is connected via Edge, 3G, WiMax, or LTE
* `unknown` - error case and the network status is unknown

```javascript
NetInfo.reachabilityIOS.fetch().done((reach) => {
  console.log('Initial: ' + reach);
});
function handleFirstReachabilityChange(reach) {
  console.log('First change: ' + reach);
  NetInfo.reachabilityIOS.removeEventListener(
    'change',
    handleFirstReachabilityChange
  );
}
NetInfo.reachabilityIOS.addEventListener(
  'change',
  handleFirstReachabilityChange
);
```

## isConnected 

Available on all platforms. Asyncronously fetch a boolean to determine internet connectivity.

```javascript
NetInfo.isConnected.fetch().done((isConnected) => {
  console.log('First, is ' + (isConnected ? 'online' : 'offline'));
});
function handleFirstConnectivityChange(isConnected) {
  console.log('Then, is ' + (isConnected ? 'online' : 'offline'));
  NetInfo.isConnected.removeEventListener(
    'change',
    handleFirstConnectivityChange
  );
}
NetInfo.isConnected.addEventListener(
  'change',
  handleFirstConnectivityChange
);
```

## Examples 

```javascript
'use strict';

var React = require('react-native');
var {
  NetInfo,
  Text,
  View
} = React;

var ReachabilitySubscription = React.createClass({
  getInitialState() {
    return {
      reachabilityHistory: [],
    };
  },
  componentDidMount: function() {
    NetInfo.reachabilityIOS.addEventListener(
      'change',
      this._handleReachabilityChange
    );
  },
  componentWillUnmount: function() {
    NetInfo.reachabilityIOS.removeEventListener(
      'change',
      this._handleReachabilityChange
    );
  },
  _handleReachabilityChange: function(reachability) {
    var reachabilityHistory = this.state.reachabilityHistory.slice();
    reachabilityHistory.push(reachability);
    this.setState({
      reachabilityHistory,
    });
  },
  render() {
    return (
      <View>
        <Text>{JSON.stringify(this.state.reachabilityHistory)}</Text>
      </View>
    );
  }
});

var ReachabilityCurrent = React.createClass({
  getInitialState() {
    return {
      reachability: null,
    };
  },
  componentDidMount: function() {
    NetInfo.reachabilityIOS.addEventListener(
      'change',
      this._handleReachabilityChange
    );
    NetInfo.reachabilityIOS.fetch().done(
      (reachability) => { this.setState({reachability}); }
    );
  },
  componentWillUnmount: function() {
    NetInfo.reachabilityIOS.removeEventListener(
      'change',
      this._handleReachabilityChange
    );
  },
  _handleReachabilityChange: function(reachability) {
    this.setState({
      reachability,
    });
  },
  render() {
    return (
      <View>
        <Text>{this.state.reachability}</Text>
      </View>
    );
  }
});

var IsConnected = React.createClass({
  getInitialState() {
    return {
      isConnected: null,
    };
  },
  componentDidMount: function() {
    NetInfo.isConnected.addEventListener(
      'change',
      this._handleConnectivityChange
    );
    NetInfo.isConnected.fetch().done(
      (isConnected) => { this.setState({isConnected}); }
    );
  },
  componentWillUnmount: function() {
    NetInfo.isConnected.removeEventListener(
      'change',
      this._handleConnectivityChange
    );
  },
  _handleConnectivityChange: function(isConnected) {
    this.setState({
      isConnected,
    });
  },
  render() {
    return (
      <View>
        <Text>{this.state.isConnected ? 'Online' : 'Offline'}</Text>
      </View>
    );
  }
});

exports.title = 'NetInfo';
exports.description = 'Monitor network status';
exports.examples = [
  {
    title: 'NetInfo.isConnected',
    description: 'Asyncronously load and observe connectivity',
    render(): ReactElement { return <IsConnected />; }
  },
  {
    title: 'NetInfo.reachabilityIOS',
    description: 'Asyncronously load and observe iOS reachability',
    render(): ReactElement { return <ReachabilityCurrent />; }
  },
  {
    title: 'NetInfo.reachabilityIOS',
    description: 'Observed updates to iOS reachability',
    render(): ReactElement { return <ReachabilitySubscription />; }
  },
];
```
NetInfo展现了在线/离线的状态信息
## reachabilityIOS 
异步决定设备是否在线和是否连接蜂窝网络。

* `none` - 设备离线
* `wifi` - 设备通过wifi连接在线，或者设备是iOS模拟器
* `cell` - 设备通过Edge, 3G, WiMax或者LTE连接
* `unknown` - 错误状态，未知网络状态

```javascript
NetInfo.reachabilityIOS.fetch().done((reach) => {
  console.log('Initial: ' + reach);
});
function handleFirstReachabilityChange(reach) {
  console.log('First change: ' + reach);
  NetInfo.reachabilityIOS.removeEventListener(
    'change',
    handleFirstReachabilityChange
  );
}
NetInfo.reachabilityIOS.addEventListener(
  'change',
  handleFirstReachabilityChange
);
```

## isConnected 

所有平台都可获得异步获取到的一个决定网络连接布尔值。

```javascript
NetInfo.isConnected.fetch().done((isConnected) => {
  console.log('First, is ' + (isConnected ? 'online' : 'offline'));
});
function handleFirstConnectivityChange(isConnected) {
  console.log('Then, is ' + (isConnected ? 'online' : 'offline'));
  NetInfo.isConnected.removeEventListener(
    'change',
    handleFirstConnectivityChange
  );
}
NetInfo.isConnected.addEventListener(
  'change',
  handleFirstConnectivityChange
);
```

## Examples 

```javascript
'use strict';

var React = require('react-native');
var {
  NetInfo,
  Text,
  View
} = React;

var ReachabilitySubscription = React.createClass({
  getInitialState() {
    return {
      reachabilityHistory: [],
    };
  },
  componentDidMount: function() {
    NetInfo.reachabilityIOS.addEventListener(
      'change',
      this._handleReachabilityChange
    );
  },
  componentWillUnmount: function() {
    NetInfo.reachabilityIOS.removeEventListener(
      'change',
      this._handleReachabilityChange
    );
  },
  _handleReachabilityChange: function(reachability) {
    var reachabilityHistory = this.state.reachabilityHistory.slice();
    reachabilityHistory.push(reachability);
    this.setState({
      reachabilityHistory,
    });
  },
  render() {
    return (
      <View>
        <Text>{JSON.stringify(this.state.reachabilityHistory)}</Text>
      </View>
    );
  }
});

var ReachabilityCurrent = React.createClass({
  getInitialState() {
    return {
      reachability: null,
    };
  },
  componentDidMount: function() {
    NetInfo.reachabilityIOS.addEventListener(
      'change',
      this._handleReachabilityChange
    );
    NetInfo.reachabilityIOS.fetch().done(
      (reachability) => { this.setState({reachability}); }
    );
  },
  componentWillUnmount: function() {
    NetInfo.reachabilityIOS.removeEventListener(
      'change',
      this._handleReachabilityChange
    );
  },
  _handleReachabilityChange: function(reachability) {
    this.setState({
      reachability,
    });
  },
  render() {
    return (
      <View>
        <Text>{this.state.reachability}</Text>
      </View>
    );
  }
});

var IsConnected = React.createClass({
  getInitialState() {
    return {
      isConnected: null,
    };
  },
  componentDidMount: function() {
    NetInfo.isConnected.addEventListener(
      'change',
      this._handleConnectivityChange
    );
    NetInfo.isConnected.fetch().done(
      (isConnected) => { this.setState({isConnected}); }
    );
  },
  componentWillUnmount: function() {
    NetInfo.isConnected.removeEventListener(
      'change',
      this._handleConnectivityChange
    );
  },
  _handleConnectivityChange: function(isConnected) {
    this.setState({
      isConnected,
    });
  },
  render() {
    return (
      <View>
        <Text>{this.state.isConnected ? 'Online' : 'Offline'}</Text>
      </View>
    );
  }
});

exports.title = 'NetInfo';
exports.description = 'Monitor network status';
exports.examples = [
  {
    title: 'NetInfo.isConnected',
    description: 'Asyncronously load and observe connectivity',
    render(): ReactElement { return <IsConnected />; }
  },
  {
    title: 'NetInfo.reachabilityIOS',
    description: 'Asyncronously load and observe iOS reachability',
    render(): ReactElement { return <ReachabilityCurrent />; }
  },
  {
    title: 'NetInfo.reachabilityIOS',
    description: 'Observed updates to iOS reachability',
    render(): ReactElement { return <ReachabilitySubscription />; }
  },
];
```