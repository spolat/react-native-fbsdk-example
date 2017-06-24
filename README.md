# React Native FBSDK


![19238557_1596848687052400_582571460_o](https://user-images.githubusercontent.com/25878632/27212633-f46f606e-5269-11e7-8cdf-6a3b560c30bd.png)



## Installation
You need to install the sdk with [npm](https://www.npmjs.com/) and configure native Android/iOS project in the react native project.
### 1. Create React Native project

First create a React Native project:
```ruby
react-native init YourApp
```

### 2. Install JavaScript packages

Install and link the react-native-fbsdk package:
```ruby
react-native install react-native-fbsdk
react-native link react-native-fbsdk
```
### Before you can run the project, follow the [Getting Started Guide](https://developers.facebook.com/docs/android/getting-started/)

#### 3 Android project

**If your react-native version is below 0.29.0**

Add an instance variable of type `CallbackManager` in class.
```java
import android.content.Intent;     // <--- import
import android.os.Bundle;

import com.facebook.CallbackManager;
import com.facebook.FacebookSdk;
import com.facebook.reactnative.androidsdk.FBSDKPackage;

public class MainActivity extends ReactActivity {
    CallbackManager mCallbackManager;
    //...
```
Register sdk package in method `getPackages()`.
```java
@Override
protected List<ReactPackage> getPackages() {
    mCallbackManager = new CallbackManager.Factory().create();
    ReactPackage packages[] = new ReactPackage[]{
        new MainReactPackage(),
        new FBSDKPackage(mCallbackManager),
    };
    return Arrays.<ReactPackage>asList(packages);
}
```
Override `onActivityResult()`.
```java
@Override
public void onActivityResult(int requestCode, int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);
    mCallbackManager.onActivityResult(requestCode, resultCode, data);
}
```

**If your react-native version is 0.29 or above**

Go to `MainApplication.java` and `MainActivity.java` under `app/src/main/java/com/<project name>/` to complete setup.

In `MainApplication.java`,

Add an instance variable of type `CallbackManager` and its getter.
```java
import com.facebook.CallbackManager;
import com.facebook.FacebookSdk;
import com.facebook.reactnative.androidsdk.FBSDKPackage;
import com.facebook.appevents.AppEventsLogger;
...

public class MainApplication extends Application implements ReactApplication {

  private static CallbackManager mCallbackManager = CallbackManager.Factory.create();

  protected static CallbackManager getCallbackManager() {
    return mCallbackManager;
  }
    //...
```

Override `onCreate()` method
```java
@Override
public void onCreate() {
  super.onCreate();
  FacebookSdk.sdkInitialize(getApplicationContext());
  // If you want to use AppEventsLogger to log events.
  AppEventsLogger.activateApp(this);
}
```

Register sdk package in method `getPackages()`.
```java
private final ReactNativeHost mReactNativeHost = new ReactNativeHost(this) {
    @Override
    protected boolean getUseDeveloperSupport() {
      return BuildConfig.DEBUG;
    }

    @Override
    protected List<ReactPackage> getPackages() {
      return Arrays.<ReactPackage>asList(
          new MainReactPackage(),
          new FBSDKPackage(mCallbackManager)
      );
    }
};
```

In `MainActivity.java`

Override `onActivityResult()` method
```java
import android.content.Intent;

public class MainActivity extends ReactActivity {

    @Override
    public void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        MainApplication.getCallbackManager().onActivityResult(requestCode, resultCode, data);
    }
    //...
```

```index.android.js
import React, { Component } from 'react';
import {
  AppRegistry,
  StyleSheet,
  Text,
  View,
  TouchableOpacity
} from 'react-native';

import FBSDK, {LoginManager} from 'react-native-fbsdk'

export default class android_facebook_sdk_example extends Component {

  _fbAuth(){
    LoginManager.logInWithReadPermissions(['public_profile']).then(function(result){
      if(result.isCancelled)
      {
        console.log('Login was cancelled');
      }
      else{
        console.log('Login was success ' + result.grantedPermissions.toString());
      }
    }, function(error){
      console.log('An error occured' + error);
    })
  }

  render() {
    return (
      <View style={styles.container}>
        <TouchableOpacity onPress={this._fbAuth()}>
        </TouchableOpacity>
      </View>
    );
  }
}
```
#### 4 Run Android project
```
react-native run-android
react-native-start

After app installation click Reload(R,R) or double press "R" key on your keyboard.
```

