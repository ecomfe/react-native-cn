---
id: testing
title: Testing
layout: docs
category: Guides
permalink: docs/testing.html
next: activityindicatorios
---

## Running Tests and Contributing

The React Native repo has several tests you can run to verify you haven't caused a regression with your PR.  These tests are run with the [Travis](http://docs.travis-ci.com/) continuous integration system, and will automatically post the results to your PR.  You can also run them locally with cmd+U in the IntegrationTest and UIExplorer apps in Xcode.  You can run the jest tests via `npm test` on the command line.  We don't have great test coverage yet, however, so most changes will still require significant manual verification, but we would love it if you want to help us increase our test coverage!

## Jest Tests

[Jest](http://facebook.github.io/jest/) tests are JS-only tests run on the command line with node.  The tests themselves live in the `__tests__` directories of the files they test, and there is a large emphasis on aggressively mocking out functionality that is not under test for failure isolation and maximum speed.  You can run the existing React Native jest tests with `npm test` from the react-native root, and we encourage you to add your own tests for any components you want to contribute to.  See [`getImageSource-test.js`](https://github.com/facebook/react-native/blob/master/Examples/Movies/__tests__/getImageSource-test.js) for a basic example.

## Integration Tests.

React Native provides facilities to make it easier to test integrated components that require both native and JS components to communicate across the bridge.  The two main components are `RCTTestRunner` and `RCTTestModule`.  `RCTTestRunner` sets up the ReactNative environment and provides facilities to run the tests as `XCTestCase`s in Xcode (`runTest:module` is the simplest method).  `RCTTestModule` is exported to JS via `NativeModules` as `TestModule`.  The tests themselves are written in JS, and must call `TestModule.markTestCompleted()` when they are done, otherwise the test will timeout and fail.  Test failures are primarily indicated by throwing an exception.  It is also possible to test error conditions with `runTest:module:initialProps:expectErrorRegex:` or `runTest:module:initialProps:expectErrorBlock:` which will expect an error to be thrown and verify the error matches the provided criteria.  See [`IntegrationTestHarnessTest.js`](https://github.com/facebook/react-native/blob/master/IntegrationTests/IntegrationTestHarnessTest.js) and [`IntegrationTestsTests.m`](https://github.com/facebook/react-native/blob/master/IntegrationTests/IntegrationTestsTests/IntegrationTestsTests.m) for example usage.

## Snapshot Tests

A common type of integration test is the snapshot test.  These tests render a component, and verify snapshots of the screen against reference images using `TestModule.verifySnapshot()`, using the [`FBSnapshotTestCase`](https://github.com/facebook/ios-snapshot-test-case) library behind the scenes.  Reference images are recorded by setting `recordMode = YES` on the `RCTTestRunner`, then running the tests.  Snapshots will differ slightly between 32 and 64 bit, and various OS versions, so it's recommended that you enforce tests are run with the correct configuration.  It's also highly recommended that all network data be mocked out, along with other potentially troublesome dependencies.  See [`SimpleSnapshotTest`](https://github.com/facebook/react-native/blob/master/IntegrationTests/SimpleSnapshotTest.js) for a basic example.

## 运行测试和贡献代码

React Native 仓库已经有一些测试用例了，你可以自行验证你发起的 PR 还能保证项目能正常回归。这些测试用例运行在[Travis](http://docs.travis-ci.com/)持续集成系统上，针对你的 PR 会自动将运行结果发布到 CI 上。你也可以本地在 Xcode 里使用 IntegrationTest 和 UIExplorer 应用，通过按 cmd+U 快捷键来回归这些测试用例。当然，你也可以命令行下使用 `npm test` 来运行 jest 测试用例。当前，测试用例覆盖率并不好，因此，对于目前大部分代码变化，仍然需要人工进行仔细地验证回归。当然，如果你乐意帮助我们增加测试用例的覆盖率，我们将会非常欢迎！

## Jest 测试

[Jest](http://facebook.github.io/jest/) 测试用例目前只是 JS 的测试用例，使用 node 在命令行下运行。测试用例文件放在它所要测试的文件所在文件夹下的 `__tests__` 文件夹里，有大量被模拟的功能并没有被测试，由于失败隔离和最大速度的缘故。你可以在 react-native 根目录下使用 `npm test` 命令运行已经存在的 React Native jest 测试用例，我们鼓励大家为你要贡献的任何组件添加你自己的测试用例。具体你可以查看这个基本的例子[`getImageSource-test.js`](https://github.com/facebook/react-native/blob/master/Examples/Movies/__tests__/getImageSource-test.js)。

## 集成测试

React Native 提供了一些基础设施，使得你可以很方便地测试集成组件，这要求你的本地组件和 JS 组件能够通过桥接方式进行通信。其中的两个主要组件是 `RCTTestRunner` 和 `RCTTestModule` 。`RCTTestRunner` 建立了 ReactNative 环境并提供一些工具方法使得测试用例能作为 Xcode 里 `XCTestCase` 测试用例来运行（`runTest:module` 是最简单的方法）。`RCTTestModule` 是通过 `NativeModules` 作为 `TestModule` 暴露给 JS 的。测试用例本身是通过 JS 写的，当测试用例执行完成必须调用 `TestModule.markTestCompleted()` 方法，否则会因为超时而导致测试用例失败。测试失败，通常主要通过抛异常来体现。你也可能需要通过使用 `runTest:module:initialProps:expectErrorRegex:`  或 `runTest:module:initialProps:expectErrorBlock:` 来测试错误条件，这时候它会期望抛出一个错误，并通过匹配给定的条件来验证错误。具体使用，你可以参考例子[`IntegrationTestHarnessTest.js`](https://github.com/facebook/react-native/blob/master/IntegrationTests/IntegrationTestHarnessTest.js) 和 [`IntegrationTestsTests.m`](https://github.com/facebook/react-native/blob/master/IntegrationTests/IntegrationTestsTests/IntegrationTestsTests.m)。


## 快照测试

一种公共类型的集成测试是快照测试。这些测试用例会渲染一个组件，并使用 `TestModule.verifySnapshot()` 方法来对比参考图像来验证屏幕的快照，该测试场景背后使用了 [`FBSnapshotTestCase`](https://github.com/facebook/ios-snapshot-test-case) 库。参考图像的录制，是通过在 `RCTTestRunner` 设置 `recordMode = YES`，然后运行测试用例。快照在32位和62位操作系统下有明显不同，以及在各种操作系统版本下，因此推荐你强制让所有测试用例在准确的配置下运行。同时也极力推荐你对所有网络数据进行模拟，以及其它潜在可能比较麻烦的依赖。具体使用例子，可以参考[`SimpleSnapshotTest`](https://github.com/facebook/react-native/blob/master/IntegrationTests/SimpleSnapshotTest.js)。

