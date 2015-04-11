Use `Navigator` to transition between different scenes in your app. To accomplish this, provide route objects to the navigator to identify each scene, and also a `renderScene` function that the navigator can use to render the scene for a given route.

To change the animation or gesture properties of the scene, provide a `configureScene` prop to get the config object for a given route. See `Navigator.SceneConfigs` for default animations and more info on scene config options.

## Basic Usage 

```html
  <Navigator
    initialRoute={{name: 'My First Scene', index: 0}}
    renderScene={(route, navigator) =>
      <MySceneComponent
        name={route.name}
        onForward={() => {
          var nextIndex = route.index + 1;
          navigator.push({
            name: 'Scene ' + nextIndex,
            index: nextIndex,
          });
        }}
        onBack={() => {
          if (route.index > 0) {
            navigator.pop();
          }
        }}
      />
    }
  />
```

## Navigation Methods 
`Navigator` can be told to navigate in two ways. If you have a ref to the element, you can invoke several methods on it to trigger navigation:

* `jumpBack()` - Jump backward without unmounting the current scene
* `jumpForward()` - Jump forward to the next scene in the route stack
* `jumpTo(route)` - Transition to an existing scene without unmounting
* `push(route)` - Navigate forward to a new scene, squashing any scenes that you could jumpForward to
* `pop()` - Transition back and unmount the current scene
* `replace(route)` - Replace the current scene with a new route
* `replaceAtIndex(route, index)` - Replace a scene as specified by an index
* `replacePrevious(route)` - Replace the previous scene
* `immediatelyResetRouteStack(routeStack)` - Reset every scene with an array of routes
* `popToRoute(route)` - Pop to a particular scene, as specified by it's route. All scenes after it will be unmounted
* `popToTop()` - Pop to the first scene in the stack, unmounting every other scene

## Navigator Object

The navigator object is made available to scenes through the `renderScene` function. The object has all of the navigation methods on it, as well as a few utilities:

* `parentNavigator` - a refrence to the parent navigator object that was passed in through props.navigator
* `onWillFocus` - used to pass a navigation focus event up to the parent navigator
* `onDidFocus` - used to pass a navigation focus event up to the parent navigator

## Props 

**configureScene** function 

Optional function that allows configuration about scene animations and gestures. Will be invoked with the route and should return a scene configuration object

```javascript
(route) => Navigator.SceneConfigs.FloatFromRight
```

**initialRoute** object 

Provide a single "route" to start on. A route is an arbitrary object that the navigator will use to identify each scene before rendering. Either initialRoute or initialRouteStack is required.

**initialRouteStack** [object] 

Provide a set of routes to initially mount the scenes for. Required if no initialRoute is provided

**navigationBar** node 

Optionally provide a navigation bar that persists across scene transitions

**navigator** object 

Optionally provide the navigator object from a parent Navigator

**onDidFocus** function 

Will be called with the new route of each scene after the transition is complete or after the initial mounting. This overrides the onDidFocus handler that would be found in this.props.navigator

**onItemRef** function 

Will be called with (ref, indexInStack) when the scene ref changes

**onWillFocus** function 

Will emit the target route upon mounting and before each nav transition, overriding the handler in this.props.navigator. This overrides the onDidFocus handler that would be found in this.props.navigator

**renderScene** function 

Required function which renders the scene for a given route. Will be invoked with the route, the navigator object, and a ref handler that will allow a ref to your scene to be provided by props.onItemRef

```javascript
(route, navigator, onRef) =>
  <MySceneComponent title={route.title} ref={onRef} /> 
```

**sceneStyle** [View#style](http://facebook.github.io/react-native/docs/view.html#style)

Styles to apply to the container of each scene

**shouldJumpOnBackstackPop** bool 

Should the backstack back button "jump" back instead of pop? Set to true if a jump forward might happen after the android back button is pressed, so the scenes will remain mounted

# 转场

使用`Navigator`可以在你的应用中的不同场景之间进行转换。为了做到这一点，我们不仅为转场器提供了标识每一个场景的路由对象，还提供了一个`renderScene`函数，转场器可以使用该函数来渲染一个给定路由的场景。

为了改变场景的动画或者手势属性，我们提供了一个`configureScene`属性来获取一个给定的路由的场景配置对象。更多默认动画和场景配置选项信息，详见`Navigator.SceneConfigs`。

## 基本用法

```html
  <Navigator
    initialRoute={{name: 'My First Scene', index: 0}}
    renderScene={(route, navigator) =>
      <MySceneComponent
        name={route.name}
        onForward={() => {
          var nextIndex = route.index + 1;
          navigator.push({
            name: 'Scene ' + nextIndex,
            index: nextIndex,
          });
        }}
        onBack={() => {
          if (route.index > 0) {
            navigator.pop();
          }
        }}
      />
    }
  />
```

## 转场方法

`Navigator`有两种转场方式。如果你持有该元素的一个引用，你可以应用一些方法到该引用上来触发转场:

* `jumpBack()`  - 后退，而无需卸载当前场景
* `jumpForward()` - 前进到路由栈中的下一个场景
* `jumpTo(route)` - 转换到一个已存在的场景，而无需卸载
* `push(route)` - 压下所有你能前进到的场景，向前转场到一个新的场景
* `pop()` - 回退，同时卸载当前场景
* `replace(route)` - 用一个新的路由替换当前的场景
* `replaceAtIndex(route, index)` - 替换指定序号场景的路由
* `replacePrevious(route)` - 替换上一个场景的路由
* `immediatelyResetRouteStack(routeStack)` - 用一串路由重置所有的场景
* `popToRoute(route)` - 弹出到route所指定的一个特定场景。所有在它后面的场景都都将被卸载
* `popToTop()` -  弹出到栈中的第一个场景，同时卸载其他所有的场景

## 转场对象

转场对象通过`renderScene`函数暴露给场景使用。该对象不仅拥有全部的转换方法，还具备了一些实用功能:

* `parentNavigator` - 父转场对象的一个引用，它是通过`props.navigator`传递进来的
* `onWillFocus` - 用以将转场聚焦事件传递到父转场对象
* `onDidFocus` -  用以将转场聚焦事件传递到父转场对象

## 属性

**configureScene** function 

可选函数，用来配置场景的动画和手势。一般会以一个路由来调用它，然后返回一个场景配置对象

```javascript
(route) => Navigator.SceneConfigs.FloatFromRight
```

**initialRoute** object 

提供了一个单一的"route"来开始转场。一个路由是一个任意的对象，在渲染前，转场器会用它来确认每一个场景。`initialRoute`或者`initialRouteStack`两者有一个是必选的

**initialRouteStack** [object] 

提供了一个路由集合来初始装载场景。如果没有提供`initialRoute`，则是必选的

**navigationBar** node 

可选地提供了一个转场条，它在场景转换的过程中依然存在

**navigator** object 

可选地提供了一个父转场对象

**onDidFocus** function 

当转场或者初始装载完成后，将会以每一个场景的新路由来调用它。这会覆盖`this.props.navigator`中的`onDidFocus`处理器

**onItemRef** function 

当场景ref发生变化时，会以`(ref, indexInStack)`这样的形式来调用它

_ps: 这里的ref应该是一个字符串标记_

**onWillFocus** function 

它覆盖了`this.props.navigator`中的处理器，并且在装载时以及每一个转场装换前，发出目标路由。这会覆盖`this.props.navigator`中的onDidFocus`处理器。

**renderScene** function 

必选函数，它负责渲染指定路由的场景。以路由、转场器对象、一个ref处理器来调用它，该ref处理器允许让你的场景的ref由`props.onItemRef`来提供

```javascript
(route, navigator, onRef) =>
  <MySceneComponent title={route.title} ref={onRef} /> 
```

**sceneStyle** [View#style](http://facebook.github.io/react-native/docs/view.html#style)

应用到每个场景容器的样式

**shouldJumpOnBackstackPop** bool 

退栈回退按钮应该用“跳”回取代弹出吗？如果按下android后退按钮后可能会发生一个前进，那设置为true，这样所有场景都会保持已装载状态。

_ps: 最新版的react-native上似乎没有`shouldJumpOnBackstackPop`这一项了_
