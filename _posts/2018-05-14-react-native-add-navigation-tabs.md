{% include post-nav.html %}

# Adding Navigation Tabs

If you've made it this far, give yourself a pat on the back. Getting these native modules working is always trying. The good news is, the fun part is now. We get to add some simple tab based navigation to the app.

We're going to tie together the react-native-vector-icons and the react-native-navigation components in this post. To get started, lets create a new file in the project called `createNavTabs.js`.

We're going to associate icons with the tabs, so we need to import the navigation and icon packages.

```javascript
import { Navigation } from "react-native-navigation";
import Icon from "react-native-vector-icons/Ionicons";
```

I'm going to pick some random icons, but feel free to look at the <a href="https://ionicframework.com/docs/ionicons/" target="_blank">link here to pick your own icons</a>.

## Loading Icons Asynchronously

When we're not specifying an icon in the markup, we're going to load them using the `getImageSource()` function. `getImageSource` takes two parameters, the name and the size. For our purproses, we'll load a 30x30 icon. Paste the following below your import statements.

```javascript
const startTabs = () => {
  Promise.all([
    Icon.getImageSource("md-home", 30),
    Icon.getImageSource("md-menu", 30)
  ]).then(sources => {
    Navigation.startTabBasedApp({
      tabs: [
        {
          screen: "sports-app.ScoreScreen",
          label: "Scores",
          icon: sources[0]
        },
        {
          screen: "sports-app.Screen2",
          label: "Screen 2",
          icon: sources[1]
        }
      ]
    });
  });
};

export default startTabs;
```

We're doing quite a lot in just a small space. First, we've kicked off an array of promises to load 2 icons from the vector icon library. When that's completed, the success case will return an array of icons in the order that we requested them.

The good stuff happens in the `Navigation.startTabBasedApp()` which takes an object that contains an array of tabs and other configuration items. For now, we'll keep it simple and create two tab items.

To make this work, we're going to do a bit of refactoring by putting our existing screen from `App.js` into its own screen. I'm going to organize this by creating a new `screens` folder off of the `src` folder (which is inline with our `components` folder).

In there, I'm going to create a file named `ScoreScreen.js` and a placeholder for Screen 2 (which we haven't developed yet) and for now we'll call it `Screen2.js`.

I literally gutted `App.js` and put nearly all of it into `ScoreScreen.js`. I changed the name of the class from `App` to `ScoreScreen`. `App.js` is now going to look like this for starters.

```javascript
import React from "react";
import { StyleSheet, Text, View } from "react-native";
import Icon from "react-native-vector-icons/Ionicons";
import { Navigation } from "react-native-navigation";
import ScoreScreen from "./src/screens/ScoreScreen";
import Screen2 from "./src/screens/Screen2";
import startTabs from "./src/createNavTabs";
```

This includes all of dependencies we need to load the tab navigation. From there, we hand off the screen display to the navigation library. So `App.js` becomes fairly dumb. It just provides the configuration tying screens to navigation and calls our `startTabs()` function.

```javascript
Navigation.registerComponent("sports-app.ScoreScreen", () => ScoreScreen);
Navigation.registerComponent("sports-app.Screen2", () => Screen2);

startTabs();
```

## One more thing!

Since we moved the ScoreScreen into the `screens` folder, we have to update the paths of our `import` statements. Those will change like so.

```javascript
import React from "react";
import { FlatList, StyleSheet, Text, View, Image } from "react-native";
import DatePicker from "react-native-datepicker";
import Toast from "react-native-root-toast";
import moment from "moment";
import axios from "../axios-sports";
import Icon from "react-native-vector-icons/Ionicons";
import ScorecardItem from "../components/ScorecardItem";
```

It's really just axios and the `ScorecardItem` component imports that needed changing, but the entire import section is there for your copy and paste convenience.

### Screen 2

For `Screen2.js`, we'll just add some placeholder stuff for now.

```javascript
import React from "react";
import { Text, View } from "react-native";

export default class Screen2 extends React.Component {
  render() {
    return (
      <View>
        <Text>Hello from Screen 2</Text>
      </View>
    );
  }
}
```

Lets fire up the app and see what it looks like.

![React Native Navigation Screen 1]({{ site.imageurl }}/ios-navigation-screen1.png)

And then we press on the Screen 2 text at the bottom and we'll see our second placeholder screen.

![React Native Navigation Screen 2]({{ site.imageurl }}/ios-navigation-screen2.png)

And we've got it! In the next lesson we'll add some functionality to the second screen.

If you need some help, please check out this commit in github which contains our application up to this point.

<a href="https://github.com/bbuchanan/react-native-sports-app/tree/2f77bb8a2d0c953541299017a2c7a689545b1c9d" target="_blank">Sports App Repository up to this point.</a>
