{% include post-nav.html %}

# Adding React Native Libraries for iOS in Xcode

Undoubtedly one of the most frustrating experiences is adding 3rd party native libraries. You're going to get all kinds of weird error messages, build issues in both Android Studio and Xcode, but stick with it. I'll offer some tips and tricks to force things to work again and provide some guidance around pitfalls.

The goal of the next few articles is to demonstrate basic navigation features in an app. It's typical for a mobile app to have more than one screen and normal for a mobile app to have several top level components that the user can jump to with one press.

First things first, lets fix our app to remove the intentional error that forced the toast notification to pop up. Do this and verify that your app displays scores again.

# Adding react-native-vector-icons

This package is handy because it contains nicely polished icons that are typical for applications. They provide icons for both Android (material design) and iOS.

[React Native Vector Icons](https://github.com/oblador/react-native-vector-icons) is the github home of this project. As usual, we're going to use yarn add to begin adding support for this project. Unfortunately, that's the easy part. We'll also need to add support to both iOS and Android via the respective native project files. This will be tough the first few times you do it, but it gets easier.

Lets get started.

`yarn add react-native-vector-icons`

Now we're going to handle adding the library to each platform. If you only need one of the two platforms, feel free to skip.

## Getting Your Native Tools Set Up

Getting this part started was outlined in the [React Native Environment article]({{ site.baseurl }}/2018-03-08-react-native-environment-setup.html). If you missed it, I suggest you read that before continuing here.

## Setting Up For iOS (Xcode)

We're going to follow the instructions for the manual setup. While there are a few icons to choose from, we're only going to use Ionicons. There are two folders to be aware of. First, the react-native-vector-icons package exists in your `node_modules` folder. Locate that folder in Finder or Windows Explorer.

![React Native Vector Icons Folder]({{ site.imageurl }}/react-native-vector-icons-folder.png)

Next locate the `ios` folder off of the project root. Note: if you don't have an `ios` or `android` folder here, you likely haven't ejected and need to perform this step before continuning.

![React Native iOS Project File]({{ site.imageurl }}/react-native-ios-project.png)

Double click this to open Xcode with this project. Once open, we're going to drag and drop the `Ionicons.ttf` file from the fonts folder that we just opened onto Xcode.

Next, locate the `Info.plist` file in your xcode project. Double click to open and at the bottom, right click and select `Add Row` and in the dropdown for the new row, select `Fonts provided by application`.

![React Native iOS Add Font]({{ site.imageurl }}/react-native-vector-icons-ios-add-fonts.png)

Next, we want to link the vector icons binary in our iOS project. Back in your Finder or Explorer, go back to your `node_modules/react-native-vector-icons` folder and locate the `RNVectorIcons.xcodeproj` file. Drag and drop this on to XCode under the `Libraries` node as shown below.

![React Native iOS Add Font Library]({{ site.imageurl }}/react-native-vector-icons-ios-libraries.png)

The last step is to `Link Binary With Libraries` under the `Build Phases` section (click the very top node on the left in your project if you're not seeing this). Click the + button.

![React Native iOS Link Library]({{ site.imageurl }}/react-native-vector-ios-build-phases.png)

Finally, make sure the project builds within Xcode. (Yes, there will be a lot of warnings, you can ignore them).

## Testing Vector Icon Library

Even though the project builds, We still want to make sure the icon library is actually working. There are two quick things to test this. First thing we need to do is import the Ionicon font. Towards the top of `App.js` next to your other imports, add this line:

`import Icon from "react-native-vector-icons/Ionicons";`

In our `render()` method, I'm simply going to add a dummy icon to see if it displays. For a list of the names that can be used with <a href="https://ionicframework.com/docs/ionicons/" target="_blank">Ionicons, see this link</a>. You can click any icon on the list and it will give you the display for each platform (iOS or Android).

For the `Icon` component, all we need to do is specify a name property. For my purposes, I'm going to use `contact-ios`.

In my `render()` function, just after the opening `<View>` tag, I'm going to add this line:

`<Icon name="ios-contact" />`

Note: you will likely need to run `yarn run ios` again to get the icon library linked into the simulator.

Once you rerun the app in the simulator, your icon should appear on the top left as shown below.

![React Native Vector Icons Test]({{ site.imageurl }}/react-native-vector-icons-test.png)

It's ever so small, but you can see a tiny contact icon in the corner. No need to tweak it, we're going to remove it. This is just a simple test to make sure things are wired up.

There is one more test. When we linked the library into the project, this gives us support for loading icons at run time. This is particularly handy because you can make decisions at runtime regarding which icons to load. This is immediately useful because typically the Android icons will want the "material design" look (usually the icon names will be prefixed with 'md-') and iOS has their own look (usually prefixed with 'ios-').

To make sure this works, we're going to call the function `Icon.getImageSource()`. This function returns a promise, just like our http get call. For now, we don't care about the result. If the application runs without an error, the function is working.

At the top of your `render()` function, add this snippet.

```javascript
Icon.getImageSource("ios-home").then(icon => {
  console.log("loaded icons");
});
```

If the vector library is bound properly, this function will complete without a red screen of death. If you get a red screen, you missed a step or did something wrong.

## Common Linking Problems

One common mistake is to have the wrong target selected in Xcode. If we go back to the project in the `Build Phases` section, the Targets listed on the left side will typically have 4 entries. In our case, they should be named `sportsapp`, `sportsappTest`, `sportsapp-tvOS` and `sportsapp-tvOSTests`. It's important that you have the `sportsapp` target selected.

For a lot more detail on linking libraries with React Native and XCode <a href="http://facebook.github.io/react-native/docs/linking-libraries-ios.html#content" target="_blank">see this link in the React Native documentation</a>.

You're half way there. In the next article, we're going to link up to Android Studio for our Android version.

If you're stuck, you can find my commit for this <a href="https://github.com/bbuchanan/react-native-sports-app/tree/404ca332aca5aa98f7fbdc1c8972a316ba0bc38f" target="_blank">react native tutorial
here</a>.

{% include footer.html %}
