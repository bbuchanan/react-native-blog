# Sports App Cleanup

In the previous article, our React Native app finally got to a point where we could load data from a remote source, put that data into our component's state, iterate over each javascript array item using the `map` function and finally pass the relevant portion of each element to our child component via React props.

After all of that, we were left with annoying warning, 'Warning: Each child in an array or iterator should have a unique "key" prop.' and a less than polished scorecard.

![react-native-scorecard-output]({{ site.imageurl }}/react-native-scorecard-output.png)

The goal of this article is to get rid of the warning and make the scorecard look just a little bit better.

### Unique "key"

Most of the time when dealing with remote data, you'll receive some sort of unique identifier. Fortunately for us, there seems to be an ID given for each game.

![react-native-sports-game-output]({{ site.imageurl }}/react-native-sports-game-output.png)

We're going to pluck that ID and add that element to our scores array.

```javascript
let scores = data.data.scoreboard.gameScore.map(score => {
  return {
    awayScore: score.awayScore,
    homeScore: score.homeScore,
    homeTeam: `${score.game.homeTeam.City} ${score.game.homeTeam.Name}`,
    awayTeam: `${score.game.awayTeam.City} ${score.game.awayTeam.Name}`,
    gameId: score.game.ID,
    location: score.game.location
  };
});
```

And now in the `map()` function call in the `render()` function, we assign our `gameId` to a special property called `key`.

```javascript
const scoreItems =
  this.state.scores != null
    ? this.state.scores.map(score => (
        <ScorecardItem
          awayTeam={score.awayTeam}
          awayScore={score.awayScore}
          homeTeam={score.homeTeam}
          homeScore={score.homeScore}
          key={score.gameId}
        />
      ))
    : null;
```

If you save this file, you should notice that the warning has disappeared.

### Making it prettier

A designer I am not, but I can at least make this symmetrical so we don't have different sized scorecards. There are a couple of ways to go about this. The first method is to explore the <a href="https://facebook.github.io/react-native/docs/text.html#numberoflines" target="_blank">numberOfLines</a> prop on the `<Text>` component. By specifying `numberOfLines` we're telling React Native to truncate text with an ellipsis should the text get too long.

Since my team names seem to be the main culprit, I'm going to add the `numberOfLines` prop to both the home team and the away team like this.

```javascript
<Text numberOfLines={1} style={styles.teamName}>
  {this.props.awayTeam}
</Text>
```

and the same to the home team.

```javascript
<Text numberOfLines={1} style={styles.teamName}>
  {this.props.homeTeam}
</Text>
```

Next, I'm going to modify the `cardItemContainer` style which wraps the outermost portion of the component and explicitly set the height and width.

```javascript
cardItemContainer: {
  borderRadius: 8,
  borderColor: "black",
  borderWidth: 1,
  margin: 12,
  alignItems: "center",
  width: 120,
  height: 100
},
```

![react-native-scorecard-fixed-output]({{ site.imageurl }}/react-native-scorecard-fixed-output.png)

We can see the text truncation is working and now my scorecards are symmetrical but I've introduced two more problems. First, my opinion is that truncating the team name isn't very useful. This name is needed in its entirety. For that, I'm going to remove the truncation and make the card even wider to accommodate the long team names. I'll set the width of the card to 150 and call it a day.

Which leads us to our second problem. If you're playing with this on the simulator, you'll notice that your scores are being clipped at the bottom and that there is no way to scroll down to see them.

## React Native List Views

We have two choices for scrollable lists. The FlatList and SectionList. The FlatList component does not try and do anything to the data other than add scrolling and it's a performant scroll (as opposed to the ScrollView component). The SectionList component, the name implies, adds sectioning to the FlatList. A SectionList would be useful for something like a contact list. Usually contact lists are sorted alphabetically and each letter is represented by a section.

![react-native-sectionlist-component]({{ site.imageurl }}/react-native-sectionlist-component.png)

For our purposes, the FlatList will do. The FlatList requires a datasource, which effectively moves our previous `map` work directly into the FlatList `renderItem` property.

Our render now looks like this.

```javascript
render() {
  return (
    <View style={styles.container}>
      {this.state.scores !== null ? (
        <FlatList
          data={this.state.scores}
          keyExtractor={item => item.gameId}
          renderItem={({ item }) => (
            <ScorecardItem
              awayTeam={item.awayTeam}
              awayScore={item.awayScore}
              homeTeam={item.homeTeam}
              homeScore={item.homeScore}
            />
          )}
        />
      ) : null}
    </View>
  );
}
```

We move the scores null check into the container itself. If the scores are not null, we render a FlatList. We specify the scores from our state in the `data` property. Our previous map logic is in the `renderItem` property. One thing to note, the name of the argument must be called `item`. If you want to get the index of the item, you could change the `renderItem` function signature to look like this:

`renderItem={(item, index) => }`

### A different way to do keys

As with our previous list rendering where we specified a key, we must do the same or risk the dreaded 'VirtualizedList: missing keys for items, make sure to specify a key property on each item or provide a custom keyExtractor.'.

This is done a little differently for FlatList. Either you have a field named 'id', which we don't, or you tell React Native where to find the id via a function. We can do this inline.

`keyExtractor={item => item.gameId}`

to map our game id as the key.

Take a look at our now scrollable list.

<video width="320" height="240" controls>
  <source src="{{ site.videourl }}/react-native-flatlist.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>

We have now introduced another small issue. The list is not tiled as it was previously. This is due to the fact that the FlatList component is now our container. A little nuance here, the FlatList doesn't want the `style` property set, but rather something called `contentContainerStyle`. Here we'll set the style previously set on the `<View>` like so:

`contentContainerStyle={styles.container}`

After saving this however, you'll get a warning that states, `flexWrap:` `wrap` is not supported with the `VirtualizedList` components. Consider using `numColumns` with `FlatList` instead.

The warning is fairly clear and we know on this particular display in portrait mode that this will fit cleanly in two columns, however, this isn't very device friendly. Smaller or larger screen or if someone flips to landscape mode, we have a problem.

For now, we'll hard code `numColumns` on the `FlatList` to 2, remove the `flexWrap` style from `conatiner` and change `flexDirection` to `column`.

In the future, we'll go into how to make device orientation and screen size decisions so that this behaves in a responsive way.

### Parting Notes

As you can see from this clean up, it isn't uncommon to play whack-a-mole with React Native sometimes. Fixing a problem, particularly a visual problem, can create more problems.

If your output isn't matching mine, please pull this commit from my repository and compare your code to mine.

https://github.com/bbuchanan/react-native-sports-app/tree/3e29a52dca8717b0b0038b962c3fe836f8e62d4e

{% include footer.html %}
