NavigatorIOS wraps UIKit navigation and allows you to add back-swipe functionality across your app.

## Routes 

A route is an object used to describe each page in the navigator. The first route is provided to NavigatorIOS as `initialRoute`:

```html
render: function() {
  return (
    <NavigatorIOS
      initialRoute={{
        component: MyView,
        title: 'My View Title',
        passProps: { myProp: 'foo' },
      }}
    />
  );
},
```

Now MyView will be rendered by the navigator. It will recieve the `route` object in the route prop, a navigator, and all of the props specified in `passProps`.

See the initialRoute propType for a complete definition of a route.

## Navigator 

A `navigator` is an object of navigation functions that a view can call. It is passed as a prop to any component rendered by NavigatorIOS.

```javascript
var MyView = React.createClass({
  _handleBackButtonPress: function() {
    this.props.navigator.pop();
  },
  _handleNextButtonPress: function() {
    this.props.navigator.push(nextRoute);
  },
  ...
});
```

A navigation object contains the following functions:

* `push(route)` - Navigate forward to a new route
* `pop()` - Go back one page
* `popN(n)` - Go back N pages at once. When N=1, behavior matches pop()
* `replace(route)` - Replace the route for the current page and immediately load the view for the new route
* `replacePrevious(route)` - Replace the route/view for the previous page
* `replacePreviousAndPop(route)` - Replaces the previous route/view and transitions back to it
* `resetTo(route)` - Replaces the top item and popToTop
* `popToRoute(route)` - Go back to the item for a particular route object
* `popToTop()` - Go back to the top item

Navigator functions are also available on the NavigatorIOS component:

```html
var MyView = React.createClass({
  _handleNavigationRequest: function() {
    this.refs.nav.push(otherRoute);
  },
  render: () => (
    <NavigatorIOS
      ref="nav"
      initialRoute={...}
    />
  ),
});
```

## Props 

**initialRoute** {component: function, title: string, passProps: object, backButtonTitle: string, rightButtonTitle: string, onRightButtonPress: function, wrapperStyle: [object Object]} 

NavigatorIOS uses "route" objects to identify child views, their props, and navigation bar configuration. "push" and all the other navigation operations expect routes to be like this:

**itemWrapperStyle** [View#style](http://facebook.github.io/react-native/docs/view.html#style)

The default wrapper style for components in the navigator. A common use case is to set the backgroundColor for every page

**tintColor** string 

The color used for buttons in the navigation bar