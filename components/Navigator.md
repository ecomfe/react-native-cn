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