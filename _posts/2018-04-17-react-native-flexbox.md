# React Native Flexbox Basics

This is by no means an exhaustive tutorial on flexbox. That's beyond the scope of this walkthrough. My goal is to learn just enough to get by. Lets jump in.

Since we've gotten beyond "Hello World", I'm also going to start linking to my commits for this so that you have the source code to reference should you get stuck or something isn't clear. <a href="https://github.com/bbuchanan/react-native-sports-app/blob/84f3878714a8f0da9fa5cdcbf5ed0d16a0466424/App.js" target="_blank"> The commit for this article can be found here.</a>

The box itself is a parent component (usually a `<View />`) and dictates how the child elements are displayed.

As you probably noticed in our Hello World so far, we're already using a flexbox. The relevant settings are:

* flex: 1
* alignItems: center
* justifyContent: center

Flex 1 tells React Native that the parent container will consume all of the available space. That's easy enough. alignItems will tell the box how the children are aligned. We picked 'center' for now, so the items will be centered vertically. justifyContent determines how the content will be distributed. Here again we're using 'center', so the content will be equal from top to bottom (unless we change our primary axis, more on that later).

To help visualize this, I've created 5 `<Image />` tags on our render function. I've replaced the `<Text />` with Hello World and pasted this code 5 times.

{% raw %}

```
<Image style={styles.ourImage}
  source={{uri: "https://www.clker.com/cliparts/I/a/V/A/7/A/blue-star-outline-md.png"}} />
```

{% endraw %}

This is blue star clip art so we can visualize what the various flexbox options do.

In our stylesheet section, I added this style

```
  ourImage: {
    height: 50,
    width: 50,
    resizeMode: "contain"
  },
```

This will contain the image to 50x50. Don't forget to include the `Image` component.

```
import { StyleSheet, Text, View, Image } from "react-native";
```

![react-native-flex-box-center](/images/react-native-flex-box-center.png).

The default axis is vertical. So our container consumes all available space, puts the children vertically and centers them. As you might guess, `alignItems` can also puts things at the start or end of the container. `flex-start` and flex-end are used to accomplish this. I changed my `alignItems` to `flex-start`.

![react-native-flex-box-flex-start](/images/react-native-flex-box-flex-start.png).

Another big feature visually is the ability to change the axis. By default, the axis is vertical, or `column`. This tells the flexbox to arrange things top to bottom. If you want to change this axis, specify `row` in the `flexDirection` setting.

I'm going to set my `justifyContent` and `alignItems` back to `center` and we'll change the `flexDirection` to `row`.

![react-native-flex-box-flexDirection](/images/react-native-flex-box-flexDirection.png).

Of course there's a whole lot more to it than that, but this should be good enough for now. As usual, there is documentation and a million tutorials out there that go into great detail. This is just a crash course.

<a href="https://facebook.github.io/react-native/docs/flexbox.html" target="_blank">Flexbox documentation</a>

### A note about external resources

A bit off topic, but including items from external sources, such as images like we did in this example, will often require that resource to be servered from a secure http server (i.e. https). If it doesn't, what can happen is that the resource will not load, the app will fail silently and you and/or your users will wonder why nothing is working. So take note that a resource on another server is serving via SSL (https).

## Need a Full Stack React Developer?

I'm your guy! I've been a contract developer for over 14 years and can help you or your company on your project. The best way to contact me is via our company page. [Yye Software](https://www.yyesoftware.com)
