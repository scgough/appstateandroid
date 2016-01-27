# appstateandroid
Plugin to allow appstate reporting in Android

#Background

I was working on a cross-platform app and we needed to report back the appstate (`active` or `background`) from the Android side of things. 

`AppState` and `AppStateIOS` didn't seem to work on Android so this was created. 
The plugin will emit a message back to the JS side of your app for the Android `onResume` (active) and `onPause` (background) liecycle events.


##Usage

###MainActivity.java

```
...
mReactInstanceManager = ReactInstanceManager.builder()
    .setApplication(getApplication())
    .setBundleAssetName("index.android.bundle")
    .setJSMainModuleName("index.android")
    .addPackage(new MainReactPackage())
    .addPackage(new AppStateAndroidPluginPackage(this))     //<- Add this
    ...  
```

###In your React-Native JS
Register a listener for the emitter

```
...
var { DeviceEventEmitter } = require('react-native');
var Subscribable = require('Subscribable');
...
var yourApp = React.createClass({
    mixins: [Subscribable.Mixin],
    ...
    componentWillMount: function(){
      ...
      this.addListenerOn(DeviceEventEmitter,
        'appStateChange',
        this._appStateAndroid);
    },
    ...
    _appStateAndroid: function(e: Event) {
        console.log("APPSTATE ANDROID = ",e);
    },
    ...
    
```

###Example Console Output
```
APPSTATE ANDROID = Object { currentAppState: "background" }
APPSTATE ANDROID = Object { currentAppState: "active" }
```

