`AppRegistry` is the JS entry point to running all React Native apps. App root components should register themselves with `AppRegistry.registerComponent`, then the native system can load the bundle for the app and then actually run the app when it's ready by invoking `AppRegistry.runApplication`.

`AppRegistry` should be `required` early in the `require` sequence to make sure the JS execution environment is setup before other modules are `required`.

## Methods 

static **registerConfig**(config: Array<AppConfig>) 

static **registerComponent**(appKey: string, getComponentFunc: Function) 

static **registerRunnable**(appKey: string, func: Function) 

static **runApplication**(appKey: string, appParameters: any) 