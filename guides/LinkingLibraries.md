Not every app uses all the native capabilities, and including the code to support all those features would impact in the binary size... But we still want to make easy to add these features whenever you need them.

不是所有的应用都需要全部的原生功能，同时包含支持所有特性的代码将会影响二进制文件的大小...但是我们仍然希望当需要这些特性的时候可以方便地使用它们。

With that in mind we exposed many of these features as independent static libraries.

出于这个考虑，我们暴露大多数特性作为独立的静态库。

For most of the libs it will be as simples as dragging two files, sometimes a third step will be necessary, but no more than that.

*All the libraries we ship with React Native live on the Libraries folder in the root of the repository. Some of them are pure JavaScript, and you just need to require it. Other libraries also rely on some native code, in that case you'll have to add these files to your app, otherwise the app will throw an error as soon as you try to use the library.*Linking Libraries

对于大部分的库，这个操作像拖动两个文件一样简单，有时第三步是必需的，但仅此而已。

*我们发布的 React Native的库都在根目录下的Libraries文件夹中。其中一部分是原生JavaScript，你只需直接调用它。其他库依赖于原生代码，在这种情况下你需要添加这类文件到你的应用中，否则在应用中使用这些库的时候将会抛出异常。*

## Here the few steps to link your libraries that contain native code 

##连接含有原生代码库的步骤

### Step 1 
### 第一步


If the library has native code, there must be a .xcodeproj file inside it's folder. Drag this file to your project on Xcode (usually under the Libaries group on Xcode);

如果某个库有原生代码，必须有一个以**. xcodeproj**为拓展名的文件在文件夹中。在Xcode中拖动这个文件到你的项目中（通常在Xcode的Libraries分组）
<div class="tutorial-mock">
  <img src="http://facebook.github.io/react-native/img/AddToLibraries.png" />
</div>

### Step 2 

Click on your main project file (the one that represents the .xcodeproj) select Build Phases and drag the static library from the Products folder insed the Library you are importing to Link Binary With Libraries

<div class="tutorial-mock">
  <img src="http://facebook.github.io/react-native/img/AddToBuildPhases.png" />
</div>

###第二步

点击你的主项目文件（以**.xcodeproj**结尾）选择**Build Phases**，将**Libraries** 里面的 **Products**文件夹中的静态文件拖动到**Link Binary With Libraries**这一栏。

### Step 3 
Not every library will need this step, what you need to consider is:

*Do I need to know the contents of the library at compile time?*

###第三步
不是每个库都需要这步，你需要确认这个：

*在编译期间，是否需要知道这些库的内容？*

What that means is, are you using this library on the native site or just in JavaScript? If you are just using it in JavaScript, you are good to go!

This step is not necessary for all libraries that we ship we React Native but `PushNotificationIOS` and `LinkingIOS`.

In the case of the `PushNotificationIOS` for example, you have to call a method on the library from your `AppDelegate` every time a new push notifiation is received.

For that we need to know the library's headers. To achieve that you have to go to your project's file, select `Build Settings` and search for `Header Search Paths`. There you should include the path to you library (if it has relevant files on subdirectories remember to make it `recursive`, like `React` on the example).

你是在原生环境还是JavaScript使用这个库？如果只是在JavaScript中使用这个库，你无需往下看。

这一步在发布的React Native所有库中，除了`PushNotificationIOS `和`LinkingIOS `，都不是必需的。

在`PushNotificationIOS `的示例中，每一个新推送通知被接收时，必须从 `AppDelegate `中调用库的方法。

因此我们需要知道库的`headers`，进入项目文件，选择`Build Settings`，找到`Header Search Paths`，填写库的路径。（如果在子目录中含有相关文件，记住选中`recursive `，如示例中的`React `）。

<div class="tutorial-mock">
  <img src="http://facebook.github.io/react-native/img/AddToSearchPaths.png" />
</div>

