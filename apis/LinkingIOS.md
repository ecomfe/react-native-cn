`LinkingIOS` gives you a general interface to interact with both, incoming and outgoing app links.

## Basic Usage 

### Handling deep links 

If your app was launched from a external url registered to your app you can access and handle it from any component you want with

```javascript
componentDidMount() {
 var url = LinkingIOS.popInitialURL();
}
```

In case you also want to listen to incoming app links during your app's execution you'll need to add the following lines to you `*AppDelegate.m`:

```java
- (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(NSString *)sourceApplication annotation:(id)annotation {
  return [RCTLinkingManager application:application openURL:url sourceApplication:sourceApplication annotation:annotation];
}
```

And then on your React component you'll be able to listen to the events on LinkingIOS as follows

```javascript
componentDidMount() {
  LinkingIOS.addEventListener('url', this._handleOpenURL);
},
componentWillUnmount() {
  LinkingIOS.removeEventListener('url', this._handleOpenURL);
},
_handleOpenURL(event) {
  console.log(event.url);
}
```

### Triggering App links 

To trigger an app link (browser, email or custom schemas) you call

```javascript
LinkingIOS.openURL(url)
```

If you want to check if any installed app can handle a given url beforehand you can call

```javascript
LinkingIOS.canOpenURL(url, (supported) => {
  if (!supported) {
    AlertIOS.alert('Can\'t handle url: ' + url);
  } else {
    LinkingIOS.openURL(url);
  }
});
```

## Methods 

static **addEventListener**(type: string, handler: Function) 

Add a handler to LinkingIOS changes by listening to the `url` event type and providing the handler

static **removeEventListener**(type: string, handler: Function) 

Remove a handler by passing the `url` event type and the handler

static **openURL**(url: string) 

Try to open the given `url` with any of the installed apps.

static **canOpenURL**(url: string, callback: Function) 

Determine wether or not the an installed app can handle a given `url` The callback function will be called with `bool supported` as the only argument

static **popInitialURL**() 

If the app launch was triggered by an app link, it will pop the link url, otherwise it will return `null`