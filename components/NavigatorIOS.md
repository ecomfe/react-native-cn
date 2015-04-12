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


## 路由

在转场中，一个路由是一个用来描述每一个页面的对象。`NavigatorIOS`的第一个路由是由`initialRoute`提供的:

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

现在，`MyView`将会被转场器所渲染。它将会接收到一个route属性所指定的`route`对象，一个`navigator`对象，以及所有在`passProps`中定义的属性。

一个路由的完整定义，请查看`initialRoute propType`。

## 转场

一个`navigator`是一个包含一系列转场函数的对象，它可以被一个视图调用。它作为一个属性被传递到任意由`NavigatorIOS`渲染的组件上。

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

一个转场对象包含以下函数：

* `push(route)` - 向前转场到一个新路由
* `pop()` - 后退一页
* `popN(n)` - 一次后退N页。当N=1时，同`pop()`
* `replace(route)` - 替换当前页的路由，并且立即加载心路由所对应的页面
* `replacePrevious(route)` -  替换上一页的路由/视图
* `replacePreviousAndPop(route)` - 替换上一页的路由/视图，并且向后转场到该页面上。
* `resetTo(route)` - 替换最顶项，并且执行`popToTop`
* `popToRoute(route)` - 回退到一个特定路由对象所对应的页面上
* `popToTop()` - 回退到最顶项

`Navigator`的所有函数同样可以用在`NavigatorIOS`组件上。

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

## 属性

**initialRoute** {component: function, title: string, passProps: object, backButtonTitle: string, rightButtonTitle: string, onRightButtonPress: function, wrapperStyle: [object Object]} 

`NavigatorIOS`使用`route`对象来确定子视图、它们的属性和导航条。"push"和其他转场操作期望路由像这样:

**itemWrapperStyle** [View#style](http://facebook.github.io/react-native/docs/view.html#style)

转场器中组件默认的包装样式。一个通用的使用案例就是设置每一个页面的`backgroundColor`

**tintColor** string 

给导航条按钮使用的颜色。
