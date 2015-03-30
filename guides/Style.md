---
id: style
title: Style
layout: docs
category: Guides
permalink: docs/style.html
next: gesture-responder-system
---

React Native doesn't implement CSS but instead relies on JavaScript to let you style your application. This has been a controversial decision and you can read through those slides for the rationale behind it.

<script async class="speakerdeck-embed" data-id="2e15908049bb013230960224c1b4b8bd" data-ratio="2" src="//speakerdeck.com/assets/embed.js"></script>

## Declare Styles

The way to declare styles in React Native is the following:

```javascript
var styles = StyleSheet.create({
  base: {
    width: 38,
    height: 38,
  },
  background: {
    backgroundColor: '#222222',
  },
  active: {
    borderWidth: 2,
    borderColor: '#00ff00',
  },
});
```

`StyleSheet.create` construct is optional but provides some key advantages. It ensures that the values are **immutable** and **opaque** by transforming them into plain numbers that reference an internal table. By putting it at the end of the file, you also ensure that they are only created once for the application and not on every render.

All the attribute names and values are a subset of what works on the web. For layout, React Native implements [Flexbox](/react-native/docs/flexbox.html).

## Using Styles

All the core components accept a style attribute

```javascript
<Text style={styles.base} />
<View style={styles.background} />
```

and also accepts an array of styles

```javascript
<View style={[styles.base, styles.background]} />
```

The behavior is the same as `Object.assign`: in case of conflicting values, the one from the right-most element will have precedence and falsy values like `false`, `undefined` and `null` will be ignored. A common pattern is to conditionally add a style based on some condition.

```javascript
<View style={[styles.base, this.state.active && styles.active]} />
```

Finally, if you really have to, you can also create style objects in render, but they are highly discouraged. Put them last in the array definition.

```javascript
<View
  style={[styles.base, {
    width: this.state.width,
    height: this.state.width * this.state.aspectRatio
  }]}
/>
```

## Pass Styles Around

In order to let a call site customize the style of your component children, you can pass styles around. Use `View.propTypes.style` and `Text.propTypes.style` in order to make sure only styles are being passed.

```javascript
var List = React.createClass({
  propTypes: {
    style: View.propTypes.style,
    elementStyle: View.propTypes.style,
  },
  render: function() {
    return (
      <View style={this.props.style}>
        {elements.map((element) =>
          <View style={[styles.element, this.props.elementStyle]} />
        )}
      </View>
    );
  }
});

// ... in another file ...
<List style={styles.list} elementStyle={styles.listElement} />
```

React Native并没有实现CSS，但是替代地依赖于javascript从而让你可以对你的应用进行自定义样式. 这是一个具有争议性的决定并且你可以通过实现它背后原理的幻灯片来阅读它.

<script async class="speakerdeck-embed" data-id="2e15908049bb013230960224c1b4b8bd" data-ratio="2" src="//speakerdeck.com/assets/embed.js"></script>

## 声明样式

在 React Native 中 声明样式的方法如下所示:

```javascript
var styles = StyleSheet.create({
  base: {
    width: 38,
    height: 38,
  },
  background: {
    backgroundColor: '#222222',
  },
  active: {
    borderWidth: 2,
    borderColor: '#00ff00',
  },
});
```

`StyleSheet.create` 构造函数是可选的，但是它具有许多关键性的优势. 它通过把值转化为参考内部表中的普通数字从而使得那些值是和 **不可改变** and **透明的** . 通过将它放置到文件的末尾处, 你也可以保证他们在你的应用中仅仅创建一次并且不需要每次都渲染.

所有的属性名和属性值都是web应用中运行的一部分. 至于布局, React Native 实现了 [Flexbox](/react-native/docs/flexbox.html).

## 使用样式

所有的核心组件都接受一个样式属性.

```javascript
<Text style={styles.base} />
<View style={styles.background} />
```

并且也接受一个样式列表的数组

```javascript
<View style={[styles.base, styles.background]} />
```

它的行为和`Object.assign`如出一辙: 为了防止值出现冲突, 最右边的元素具有最高的优先级并且像 `false`, `undefined` 和 `null` 这样非法的值将会被忽略. 一种普遍的模式就是基于一些条件条件性地增加样式.

```javascript
<View style={[styles.base, this.state.active && styles.active]} />
```

最后, 如果你非要这么做, 你也可以在渲染的时候创建一个样式对象, 但是并不鼓励这样做. 最好的方式是将他们在数组定义时放在最后.

```javascript
<View
  style={[styles.base, {
    width: this.state.width,
    height: this.state.width * this.state.aspectRatio
  }]}
/>
```

## 分发样式

为了能够让一个访问站点定制你的子样式组件，你可以对你的样式进行分发. 为了确保你的样式能够被分发，你可以使用 `View.propTypes.style` 和 `Text.propTypes.style` .

```javascript
var List = React.createClass({
  propTypes: {
    style: View.propTypes.style,
    elementStyle: View.propTypes.style,
  },
  render: function() {
    return (
      <View style={this.props.style}>
        {elements.map((element) =>
          <View style={[styles.element, this.props.elementStyle]} />
        )}
      </View>
    );
  }
});

// ... in another file ...
<List style={styles.list} elementStyle={styles.listElement} />
```
