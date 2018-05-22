{% include post-nav.html %}

# Adding Sports Statistics

This next couple of posts are going to create our second screen that is a placeholder at the moment. We're not going to learn a lot new as it pertains to React Native, but we're setting ourselves up to have a little more fun with some third party user interface components. Consider this article a quick review as we stub out a way to display sports statistics. We're essentially reviewing what we know and then we're going to tie it all together with some prettiness. Lets get started ...

## Housekeeping Items

The first thing we're going to do is rename `Screen2.js` to `StatsScreen.js`. Some fallout tasks to that is changing the `createNavTabs.js` to point to `StatsScreen` now (instead of Screen2). We'll also change the label on the tab from Screen2 to Stats.

```javascript
{
  screen: "sports-app.StatsScreen",
  label: "Stats",
  icon: sources[1]
}
```

In `App.js` change the import statement for Screen2 to

`import StatsScreen from "./src/screens/StatsScreen";`

And change the `registerComponent()` for Screen2 to

```javascript
Navigation.registerComponent("sports-app.StatsScreen", () => StatsScreen);
```

The remaining pieces of work involve creating a screen that will load our sports stats.

## Sports Stats Screen - Getting Data

If you remember back to when we first starting loading data from MySportsFeed, we set up axios to pull using our authorization key. The great thing about axios is that we were able to tuck away this authorization header and the base URL that forms every request. This
allows us to make short work of getting additional data from MySportsFeed.

The screenshot below shows where we'll be retrieving the data from.

![React Native MySportFeed API Stats]({{ site.imageurl }}/my-sports-api-react-native.png)

## Sports Stats Screen - Processing Data

I'll walk through each change to the file from top to bottom, so we'll start with the imports.

Open the newly renamed `StatsScreen.js`.

We're adding a Picker component which is a React Native component, just like a Text, View, etc. We're also going to import our axios export.

```javascript
import React from "react";
import { Text, View, Picker } from "react-native";
import axios from "../axios-sports";
```

Next, change the class name from Screen2 to StatsScreen.

```javascript
export default class StatsScreen extends React.Component {
```

Just like our other screen, we're going to have a local state that holds information like the current team selected, their stats, if there was an error, etc. Just inside of the class, lets initialize our state.

```javascript
state = {
  currentTeam: "det",
  stats: null,
  hadError: false,
  errorMessage: ""
};
```

`currentTeam` is going to hold a selection from the Picker. This will be used when we load the stats. `stats` will hold our loaded stats (if loading is successful), otherwise, `hadError` and `errorMessage` will become important.

We're going to hook into the component lifecycle method `componentDidMount` once again to load some initial stats.

```javascript
componentDidMount() {
  this.loadStats();
}
```

Now for the heart of the functionality, our stat loading function.

```javascript
loadStats = () => {
  axios
    .get(`cumulative_player_stats.json?team=${this.state.currentTeam}`)
    .then(data => {
      let stats = data.data.cumulativeplayerstats.playerstatsentry.map(p => {
        return {
          firstName: p.player.FirstName,
          lastName: p.player.LastName,
          battingAverage: p.stats.BattingAvg["#text"],
          homeruns: p.stats.Homeruns["#text"]
        };
      });

      this.setState({
        stats
      });
    })
    .catch(err => {
      this.setState({
        scores: null,
        hadError: true,
        errorMessage: err.message
      });
    });
};
```

MySportsFeed really puts stuff in there good and deep, but they're very thorough, which is why we like them. After all of that, I'm just going to pluck out the player name, his batting average and number of homeruns. You're welcome to add any stats you'd like. Once we're extracted what we wanted, we're going to put the stats into the state so that they can be rendered (this will done in the next article).

## Loading Team Stats Dynamically

Our last bit of fun for this article is to add support in the user interface to change the team based on user selection. We'll use the `<Picker />` component we added to our import earlier and a `<Picker />` component exposes a `onValueChange` event hook. We'll subscribe to that with our own function which will update the state and reload the statistics based on the user's selection. Our handler looks like this:

```javascript
teamSelectionChanged = value => {
  this.setState(
    {
      currentTeam: value
    },
    () => this.loadStats()
  );
};
```

It's quick and easy, but it looks a little different than previous `setState` calls because in this version, we utilize a callback that is passed in the second argument. This callback is called when the state has been updated. We could have just as easily added an argument to `loadStats()` but I wanted to keep the team synchronized and in one place.

## Adding a React Native Picker

Finally in our `render()` function, we're going to change our `<View>` to do a lot more than just return some dummy text. We're going to add the `Picker` with some hard coded teams. The `Picker` will also wire up our `ValueChange` event.
{% raw %}

```javascript
render() {
  return (
    <View>
      <Picker
        selectedValue={this.state.currentTeam}
        style={{ height: 25, width: 100 }}
        onValueChange={(value, index) => this.teamSelectionChanged(value)}
      >
        <Picker.Item label="Tigers" value="det" />
        <Picker.Item label="Braves" value="atl" />
        <Picker.Item label="Red Sox" value="bos" />
        <Picker.Item label="Rays" value="tam" />
        <Picker.Item label="Mariners" value="sea" />
      </Picker>
    </View>
  );
}
```

{% endraw %}
The React Native picker `onValueChange` will pass two arguments. The selected value, which is what is in each `<Picker.Item> value` tag and the display index. We don't need the display index of item, but I thought I'd point it out anyway.

We're only passing the `value` to the `this.teamSelectionChanged()` function. Since this is wired up, we should be able to see our stats. I'm going to fire up the React Native Debugger. If you haven't heard of React Native Debugger, it's a great tool that you really need to know about now. Stop everything and read the section [Analyzing State In the React Native Debugger](2018-04-20-react-native-state.md) where I give a quick overview of how to install and use React Native Debugger.

And here we see our hometown Atlanta Braves stats in our state. Thankfully Ronald Acuna isn't batting 0.000 this year.

![React Native State Sports Stats]({{ site.imageurl }}/react-native-state-sports-stats.png)

It ain't pretty, but we've done all of the heavy lifting necessary now to put a pretty face on this screen, which will happen in the next set of articles.

<a href="https://github.com/bbuchanan/react-native-sports-app/tree/dd210644577f387af0513b1da31fdc7a7f62e8df" target="_blank">React Native Sports App Tutorial up to this point.</a>

{% include footer.html %}
