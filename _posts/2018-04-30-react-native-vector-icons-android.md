{% include post-nav.html %}

# Adding React Native Libraries for Android Studio

In our last article we focused on adding the react-native-vector-icons library to our iOS version of the application. This article will focus specifically on Android. If you missed it, you will need to install the [React Native Vector Icons](https://github.com/oblador/react-native-vector-icons) package:

`yarn add react-native-vector-icons`

## Getting Your Native Tools Set Up

Getting this part started was outlined in the [React Native Environment article]({{ site.baseurl }}/2018-03-08-react-native-environment-setup.html). If you missed it, I suggest you read that before continuing here.

## Setting Up For Android Studio

We're going to follow the instructions for the manual setup just like we did for iOS. Again, we're only using Ionicons. There are two folders to be aware of. First, the react-native-vector-icons package exists in your `node_modules` folder. Locate that folder in Finder or Windows Explorer.

![React Native Vector Icons Folder]({{ site.imageurl }}/react-native-vector-icons-folder.png)

Next locate the `android` folder off of the project root. Note: if you don't have an `ios` or `android` folder here, you likely haven't ejected and need to perform this step before continuning.

Open Android Studio and click `open`. Navigate to the `android` folder in the `sports-app` project and click the `Open` button. If this is the first time you've run Android Studio, you may be prompted to upgrade some modules. Agree to these (unless you have reasons not to).

We are building with Gradle, so open the `android/app/build.gradle` file in Android Studio and add the following lines:

```
project.ext.vectoricons = [
        iconFontNames: [ 'Ionicons.ttf' ]

apply from: "../../node_modules/react-native-vector-icons/fonts.gradle"
```

For Android, you have to start the emulator before you run the `yarn start android` command.

Another note, it is common for me to get this ugly error after running `yarn start android`.

```
* What went wrong:
A problem occurred configuring root project 'sportsapp'.
> Could not resolve all files for configuration ':classpath'.
   > Could not find com.android.tools.build:gradle:3.0.1.
     Searched in the following locations:
         https://jcenter.bintray.com/com/android/tools/build/gradle/3.0.1/gradle-3.0.1.pom
         https://jcenter.bintray.com/com/android/tools/build/gradle/3.0.1/gradle-3.0.1.jar
```

The solution is to open the `build.gradle` at the project level (i.e. `sportsapp` in this case) and add the line `google()` underneath `buildscript` -> `repositories`.

```
buildscript {
    repositories {
        google()
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:3.0.1'

        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}
```

## Testing Vector Icon Library

If you skipped the last tutorial because you wanted to focus on Android only, we performed two tests to make sure things worked. Even though the project builds, there are two quick things to test.

First thing we need to do is import the Ionicon font. Towards the top of `App.js` next to your other imports, add this line:

`import Icon from "react-native-vector-icons/Ionicons";`

In our `render()` method, I'm simply going to add a dummy icon to see if it displays. For a list of the names that can be used with <a href="https://ionicframework.com/docs/ionicons/" target="_blank">Ionicons, see this link</a>. You can click any icon on the list and it will give you the display for each platform (iOS or Android).

For the `Icon` component, all we need to do is specify a name property. For my purposes, I'm going to use `md-contact`, for material design since this is our Android version.

In my `render()` function, just after the opening `<View>` tag, I'm going to add this line:

`<Icon name="md-contact" />`

Now we can see icon, albeit tiny, but it works.

![React Native Vector Icons Android]({{ site.imageurl }}/react-native-vector-icons-android.png)

Next we want to hook up dynamic loading support. This is particularly handy because you can make decisions at runtime regarding which icons to load. This is immediately useful because typically the Android icons will want the "material design" look (usually the icon names will be prefixed with 'md-') and iOS has their own look (usually prefixed with 'ios-').

In `android/settings.grade` add these lines to the bottom of the file:

```
include ':react-native-vector-icons'
project(':react-native-vector-icons').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-vector-icons/android')
```

Back in your `android/app/build.gradle` which we edited previously, look for the `dependencies` section and at the bottom of that section add:

`compile project(':react-native-vector-icons')`

Finally, in `MainApplication.java`, we'll need to import the package. Where the other `import` statements are in the file, add this to the end of the `imports`

`import com.oblador.vectoricons.VectorIconsPackage;`

Look for the `getPackages()` function. Add the line `new VectorIconsPackage()` so that your `getPackages()` function looks like this.

```Java
@Override
protected List<ReactPackage> getPackages() {
  return Arrays.<ReactPackage>asList(
      new MainReactPackage(),
      new VectorIconsPackage()
  );
}
```

To make sure this works, we're going to call the function `Icon.getImageSource()`. This function returns a promise, just like our http get call. For now, we don't care about the result. If the application runs without an error, the function is working.

At the top of your `render()` function, add this snippet.

```javascript
Icon.getImageSource("md-home").then(icon => {
  console.log("loaded icons");
});
```

If the vector library is bound properly, this function will complete without a red screen of death. If you get a red screen, you missed a step or did something wrong. If you have your debugger running, you'll see the "loaded icons" text output in the console log.

<a href="https://github.com/bbuchanan/react-native-sports-app/tree/315a450b36091b471d2f04ccf5ef1d856b74c092" target="_blank">This commit will have both iOS and Android support loaded, so if you need a little help or something to compare against, go here.</a>

{% include footer.html %}
