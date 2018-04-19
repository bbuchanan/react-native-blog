{% include post-nav.html %}

# Rendering Scores Using React Components

For this article, we're simply going to get an introduction to styles and pull together some of the knowledge we have about flexbox in React Native. We're going to make a little scorecard with hardcoded data for now. Subsequent articles will wire up the downloaded data and introduce you to <a href="https://reactjs.org/docs/components-and-props.html" target="_blank">React props</a>.

If you get stuck, <a href="https://github.com/bbuchanan/react-native-sports-app/tree/91fb39024126be0b3ba3809d0b3360ff025f4e3d" target="_blank">the commit up until this point can be found here</a>.

### Create React Native Components

A component is really nothing more than a class that derives from the Component object within React. This is an essential building block of both React JS and React Native. We're going to create a component by creating a folder named `Components` underneath our `src` directory. After that, we're going to create a Javascript file named `ScorecardItem.js`.

![react-native-component-scorecard]({{ site.imageurl }}/react-native-scorecard-component.png)

The first thing we need to do is import some things from both react and react-native.

```javascript
import React, { Component } from "react";
import { Text, View, StyleSheet } from "react-native";
```

We always need React, but in addition to this, since we're creating our own component, we'll have to import the Component class using the curly braces.

We're going to use a `<Text />`, a `<View />` and a `<StyleSheet />` to get our layout started.

The first thing to do is to create a class that extends the Component class. Every single component must implement the `render()` function and return jsx. Don't worry if you're not too familiar with jsx yet, we won't need to know too much about it. For our purposes, this is HTML-like with some bindings.

A bare bones component would look like this:

```javascript
class ScorecardItem extends Component {
  render() {
    return <View />;
  }
}

export default ScorecardItem;
```

You could now include this component as is within other components, including your app component. This `render()` function does nothing but return an empty view, so you wouldn't see anything exciting, so lets change that.

Our final product is going to look like the image below and I'm going to walk you through step-by-step how this happens.

![react-native-component-render]({{ site.imageurl }}/react-native-component-render.png)

While this is duplicated over and over on the screen, this is the same component instantiated several times with hard coded values. The goal is to get it rendering dynamically from the sports score data source, but first we're going to focus on styling our scorecard. Lets start with something simple in our render function.

```javascript
  render() {
    return (
      <View>
        <View>
          <View>
            <Text>Detroit</Text>
          </View>
          <Text>8</Text>
          <View>
            <Text>Minnesota</Text>
          </View>
          <Text>2</Text>
        </View>
      </View>
    );
  }
```

There are a lot of `<View />` tags for sure, but I'm going to use those containers to apply styling. Here is how we look without styles (the code above).

![react-native-component-no-styles]({{ site.imageurl }}/react-native-component-no-styles.png)

### Creating and Applying React Native Styles

As mentioned in a previous post, styles in React Native resemble css a little bit, but they're not full blown css styles and the syntax is slightly different. For example, there are no dashes in React Native styles, the names are camel case.

Since we're imported the `StyleSheet` namespace, we can use the `create()` function to register our styles within this component. I like to put my styles on top, before the component class declaration so that designer types don't have to dig through anything to find the styles. They're always on top.

The first style I want to create is the rounded rectangle around the scorecard. The `create()` function takes an array of styles. To get started, simply call `create()` like so:

```javascript
const styles = StyleSheet.create({
  cardItemContainer: {
    borderRadius: 8,
    borderColor: "black",
    borderWidth: 1,
    margin: 12,
    alignItems: center
  }
});
```

Phew, that's a lot. This is sitting around the entire score. The first three items, `borderRadius`, `borderColor` and `borderWidth` are all responsible for drawing the rounded rectangle around the scorecard. I've set a `margin` of 12 to give some space in between each card. And finally, since this is all in a flexbox, I'm asking for the child items to be centrally aligned.

The next thing I want to accomplish is I want space between the border itself and the text that's going to be rendered inside of each scorecard. I'm going to add another style called `scoreContainer` and set a margin of 3.

To add another style, you just add to the array like so.

```javascript
const styles = StyleSheet.create({
  cardItemContainer: {
    borderRadius: 8,
    borderColor: "black",
    borderWidth: 1,
    margin: 12,
    alignItems: center
  },
  scoreContainer: {
    margin: 3
  }
});
```

Finally, I simply want to set the font sizes for the score number and the team name, so my final stylesheet looks like this:

```javascript
const styles = StyleSheet.create({
  cardItemContainer: {
    borderRadius: 8,
    borderColor: "black",
    borderWidth: 1,
    margin: 12,
    alignItems: "center"
  },
  scoreContainer: {
    margin: 3
  },
  teamName: {
    fontSize: 14,
    textAlign: "center"
  },
  teamScore: {
    fontSize: 24,
    textAlign: "center",
    fontWeight: "bold"
  }
});
```

### Applying styles

Creating the stylesheet allows you to reuse styles, of course, but it also makes them apply cleanly within the code, just as it does with HTML and css. To apply a style we attach the style name to the style attribute.

For our top level view, we wanted the `cardItemContainer` which contains our border and margin. I want the area housing the actual score to have some space (margin: 3) between the border and text and then I want to apply some font styling for the team name and the score.

Now my render function looks like this:

```javascript
render() {
  return (
    <View style={styles.cardItemContainer}>
      <View style={styles.scoreContainer}>
        <View>
          <Text style={styles.teamName}>Detroit</Text>
        </View>
        <Text style={styles.teamScore}>8</Text>
        <View>
          <Text style={styles.teamName}>Minnesota</Text>
        </View>
        <Text style={styles.teamScore}>2</Text>
      </View>
    </View>
  );
}
```

To see this rendered inside of your application, go back to the `App.js` file and include your shiney new component like this.

`import ScorecardItem from "./src/components/ScorecardItem";`

In your render function, create a number of them like this.

```javascript
  render() {
    return (
      <View style={styles.container}>
        <ScorecardItem />
        <ScorecardItem />
        <ScorecardItem />
        <ScorecardItem />
      </View>
    );
  }
```

Your output should match mine shown at the beginning of the article. If you're having trouble, you can always pull this commit from here.

https://github.com/bbuchanan/react-native-sports-app/tree/91fb39024126be0b3ba3809d0b3360ff025f4e3d

{% include footer.html %}
