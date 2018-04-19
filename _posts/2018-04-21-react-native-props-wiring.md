{% include post-nav.html %}

# Wiring up Data via Props

We have written a lot of boilerplate for sure, but hopefully you can see the flexibility and power in this.

Now is the time for us to take the remote data and apply it to our component. <a href="https://reactjs.org/docs/components-and-props.html" target="_blank">React props</a> are a handy way to pass data from parent to child. To go the other way requires a completely different technique using what is called a store. Redux is the package commonly used to handle this scenario. Redux is a huge subject and is out of scope for now.

If you get stuck, <a href="https://github.com/bbuchanan/react-native-sports-app/tree/9019a38813f7e8cd7c7edc31944c6e43fab49440" target="_blank">the commit up until this point can be found here</a>.

### Props Are Dynamic

Like Javascript itself, props are a "nod and a handshake" between parent and child components. It takes a tool lke Typescript to enforce some typing. We'll do some Typescript basics later on. For now, we're just going to take the data we've received and render it in our ScorecardItem component.

The first thing we're going to do is modify the ScorecardItem component to render these props. All of the hard coded values in this component are going to be changed to render props.

We are a stateful component, so props are accessed via `this` just like we did with `state` in the last article. I'm going to keep the prop names similar to what I already have defined in my `scores` object in `App.js`. My `render()` function now looks like this.

```javascript
render() {
  return (
    <View style={styles.cardItemContainer}>
      <View style={styles.scoreContainer}>
        <View>
          <Text style={styles.teamName}>{this.props.awayTeam}</Text>
        </View>
        <Text style={styles.teamScore}>{this.props.awayScore}</Text>
        <View>
          <Text style={styles.teamName}>{this.props.homeTeam}</Text>
        </View>
        <Text style={styles.teamScore}>{this.props.homeScore}</Text>
      </View>
    </View>
  );
```

The component is ready to accept these props, now it's time to deliver them from the parent component. Recall that we already have the data we need loaded into the state. We want to now iterate over the array of scores and create a ScorecardItem for each score.

## Map

If you're not familiar with `map()`, it's function available on a Javascript array that iterates over every element and provides a new array. If you're a C# developer, think of `map()` as roughly equivalent to `Select()` in Linq. For our purposes, we're going to transform this array from data to a component with props.

```javascript
const scoreItems =
  this.state.scores != null
    ? this.state.scores.map(score => (
        <ScorecardItem
          awayTeam={score.awayTeam}
          awayScore={score.awayScore}
          homeTeam={score.homeTeam}
          homeScore={score.homeScore}
        />
      ))
    : null;
return <View style={styles.container}>{scoreItems}</View>;
```

There is a lot happening for a single statement. This first thing we're checking is for `null`. If the scores haven't been loaded yet, don't iterate, but rather assign `null` to the scoreItems variable. If we're not null, we're going to invoke `map()` which will iterate over every score. Each element in the array will be assigned to a variable named `score` which is the name immediately before the fat arrow. We assign each score element to our props for the ScorecardItem component.

The last thing we need to do is actually output the contents of the `scoreItems` variable. Since we're producing proper jsx, we can simply output the value as is, but it has to wrapped in a `<View>` tag as shown in the return statement.

### Drumroll please ...

The output looks like this:

![react-native-scorecard-output]({{ site.imageurl }}/react-native-scorecard-output.png)

Well that's kind of ugly, isn't it? In the next article we'll go about making this a little more aesthetically pleasing. We'll also address the warning that was produced at the bottom.

{% include footer.html %}
