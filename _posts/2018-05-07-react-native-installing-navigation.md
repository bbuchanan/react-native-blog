{% include post-nav.html %}

# Installing React Native Navigation (iOS)

After installing the React Native Vector Icons, you're probably aware that this isn't always a smooth process, but once it's done, it usually just works fine. So lets get started.

`yarn add react-native-navigation`

We'll start with installation on iOS. Fire up Xcode and right click on the `Libraries` node and select `Add Files to sportsapp`. Navigate to the `react-native-navigation` folder under our project's `node_modules` folder. Add `./node_modules/react-native-navigation/ios/ReactNativeNavigation.xcodeproj` file.

Next, select the top level node of the project and click `Build Phases`. Like we did with `libRNVectorIcons`, add the react native navigation library as well.

![React Native Navigation Library]({{ site.imageurl }}/ios-add-react-native-navigation.png)

Next, select the `Build Settings` section on top and then scroll to the `Header Search Paths`. Add `$(SRCROOT)/../node_modules/react-native-navigation/ios` as shown below.

![React Native Navigation iOS Search Paths]({{ site.imageurl }}/ios-add-react-native-navigation-search-paths.png)

Finally, in Xcode, open the `AppDelegate.m` file and replace the contents with this.

```objc
#import "AppDelegate.h"
#import <React/RCTBundleURLProvider.h>

#import "RCCManager.h"

#import <React/RCTRootView.h>

@implementation AppDelegate

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
  NSURL *jsCodeLocation;
#ifdef DEBUG
  jsCodeLocation = [[RCTBundleURLProvider sharedSettings] jsBundleURLForBundleRoot:@"index" fallbackResource:nil];
#else
  jsCodeLocation = [[NSBundle mainBundle] URLForResource:@"main" withExtension:@"jsbundle"];
#endif

  self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];
  self.window.backgroundColor = [UIColor whiteColor];
  [[RCCManager sharedInstance] initBridgeWithBundleURL:jsCodeLocation launchOptions:launchOptions];
  return YES;
}

@end
```

Select `Product->Build` in Xcode and make sure your program still builds. If so, congratulations, you're half way there. In the next tutorial we're going to add the react-native-navigation package to Android Studio and then we'll implement some basic navigation and some additional features to our program to take advantage of the ability to navigate.

<a href="https://github.com/bbuchanan/react-native-sports-app/tree/f1ff1087ba089fa98d119262e11d4717d12c8576" target="_blank">This commit will have the iOS support, so if you need a little help or something to compare against, go here.</a>

{% include footer.html %}
