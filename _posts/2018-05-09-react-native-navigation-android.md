{% include post-nav.html %}

# Installing React Native Navigation (Android)

In our previous tutorial, we installed native navigation support in Xcode. Now we're going to apply the same library to Android using Android Studio. Load up the project in Android Studio and lets get started. If you skipped the iOS tutorial, please make sure you've installed the react-native-navigation package using the command below.

`yarn add react-native-navigation`

Add the following to `settings.gradle`

```
include ':react-native-navigation'
 project(':react-native-navigation').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-navigation/android/app/')
```

Note: you should already see our react-native-vector-icons referenced in this file. Add the navigation items from above below this.

Now open `build.gradle` from the app level.

Change the SDK and build tools version from 23 to 25.

```
android {
    compileSdkVersion 25
    buildToolsVersion "25.0.1"
```

and then towards the bottom of the same file, add these lines to the `dependencies` node. Once again, you should see our referencing the react-native-vector-icons and that's how you know you're in the right place.

Your `dependencies` node should now look like this.

```
dependencies {
    compile fileTree(dir: "libs", include: ["*.jar"])
    compile "com.android.support:appcompat-v7:23.0.1"
    compile "com.facebook.react:react-native:+"  // From node_modules
    compile project(':react-native-vector-icons')
    compile "com.facebook.react:react-native:+"
    compile project(':react-native-navigation')
}
```

Next we need to modify `MainActivity.java`. Our current file looks like this:

```java
package com.sportsapp;

import com.facebook.react.ReactActivity;

public class MainActivity extends ReactActivity {

    /**
     * Returns the name of the main component registered from JavaScript.
     * This is used to schedule rendering of the component.
     */
    @Override
    protected String getMainComponentName() {
        return "sportsapp";
    }
}
```

It needs to be changed to look like this:

```java
package com.sportsapp;

import com.facebook.react.ReactActivity;
import com.reactnativenavigation.controllers.SplashActivity;

public class MainActivity extends SplashActivity {
    /**
     * Returns the name of the main component registered from JavaScript.
     * This is used to schedule rendering of the component.
     */
    @Override
    protected String getMainComponentName() {
        return "sportsapp";
    }
}
```

Lastly, we need to update `MainApplication.java`. The first thing to add is to add this import and modify the `MainApplication` class declaration like so:

```java
 import com.reactnativenavigation.NavigationApplication;

 public class MainApplication extends NavigationApplication {
```

Somewhere in the class, add this method.

```java
@Override
     public boolean isDebug() {
         // Make sure you are using BuildConfig from your own application
         return BuildConfig.DEBUG;
     }
```

At the bottom of the class, add this method:

```java
@Override
     public List<ReactPackage> createAdditionalReactPackages() {
         return getPackages();
     }
```

<a href="https://github.com/bbuchanan/react-native-sports-app/tree/d811fc05e5a5f3fb6ffd6087123ee6a6fe885a3b" target="_blank">This commit will have Android support, so if you need a little help or something to compare against, go here.</a> Sometimes Android Studio can be
finicky.

And that should be it.

In the next article, we'll add some basic tabs to the bottom of the app to switch between screens.

{% include footer.html %}
