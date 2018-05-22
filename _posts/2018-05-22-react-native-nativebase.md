{% include post-nav.html %}

# NativeBase - An Introduction

I mentioned way back in the [React Native Hello World article](2018-04-16-react-native-hello-world.md) that we would be using a 3rd party library called NativeBase. I think most will agree that React Native's UI component support is minimal and NativeBase is one of the better attempts at rounding out that offering. They provide a number of modern UI components to greatly improve the user experience.

On top of a good offering, the documentation is also pretty good. The examples they provide get me by most of the time. Anything that isn't too obvious can usually be resolved by looking at the source code, which is clean and discoverable. There is also a fairly active community here: <a href="http://discuss.nativebase.io/" target="_blank">NativeBase Forum</a>.

It was mentioned at the end of the last article, that the goal here is to pretty up our sports statistics screen.

Here's what we're going for ...

<video width="640" height="480" controls>
  <source src="{{ site.videourl }}/react-native-stats.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>

As you're probably used to by now, lets get started by installing the NativeBase package.

`yarn add native-base`

There are going to be a lot of changes. They're all confined to the `StatsScreen.js` file, but as usually, if you get lost, grab the file from my github page for this commit. I'll link to it at the end of the article.

Here we go ...

Our import from `react-native` is going to change a bit. Replace the current import with this import.

```javascript
import { StyleSheet, Text, Picker, Image } from "react-native";
```

The big import is going to come from `native-base`. Add these lines to your import.

```javascript
import {
  Container,
  Header,
  View,
  DeckSwiper,
  Card,
  CardItem,
  Left,
  Body
} from "native-base";
```

and we're also going to import the vector icon library.

```javascript
import Icon from "react-native-vector-icons/Ionicons";
```

So far all of our styles have been inline, but we're going to break some of them out into a `StyleSheet`. We have a couple of items in the stats, a name and the stats themselves. I'm going to style those.

Add this:

```javascript
const styles = StyleSheet.create({
  playerNameText: {
    fontSize: 18,
    textAlign: "center",
    fontWeight: "bold"
  },
  statText: {
    fontWeight: "bold"
  }
});
```

## Teams

For a little bit of aesthetics, I've decided to associate a team logo with each team in our list. I've converted the selected team into an array of teams. After the `class` declaration, I added the following:

```javascript
teams = [
  {
    name: "Atlanta Braves",
    code: "atl",
    logo:
      "https://mk0teamcolorcodtgc6i.kinstacdn.com/wp-content/uploads/2017/05/atlanta_braves_logo_2018-768x312.png"
  },
  {
    name: "Detroit Tigers",
    code: "det",
    logo:
      "https://mk0teamcolorcodtgc6i.kinstacdn.com/wp-content/uploads/2014/05/tigers_logo.jpg"
  },
  {
    name: "Boston Red Sox",
    code: "bos",
    logo:
      "https://mk0teamcolorcodtgc6i.kinstacdn.com/wp-content/uploads/2017/05/boston_red_sox_logo.png"
  },
  {
    name: "Tampa Bay Rays",
    code: "tb",
    logo:
      "https://mk0teamcolorcodtgc6i.kinstacdn.com/wp-content/uploads/2014/05/rays_logo.jpg"
  },
  {
    name: "Seattle Mariners",
    code: "sea",
    logo:
      "https://mk0teamcolorcodtgc6i.kinstacdn.com/wp-content/uploads/2014/05/mariners_logo.jpg"
  }
];
```

Our `<Picker />` is going to bind to this and update our state. When we iterate over the statistics, we'll be able to put the team's logo into the card.

In our `state` object, I'm changing the `currentTeam` from "det" to `this.teams[0]`. This puts the first item in the teams array as the default selected item.

Since `currentTeam` is now an object, we'll need to change our get call to `axios` to only send the "code" item of the object.

```javascript
axios.get(`cumulative_player_stats.json?team=${this.state.currentTeam.code}`);
```

For some additional aesthetics, I'm going to assign an image to each player. There isn't a quick and easy way to get a player's image, so I've hardcoded an action shot I found and I'm going to hardcode it for now.

The `return` call for the stats object now looks like this:

```javascript
return {
  firstName: p.player.FirstName,
  lastName: p.player.LastName,
  battingAverage: p.stats.BattingAvg["#text"],
  homeruns: p.stats.Homeruns["#text"],
  image:
    "https://www.gannett-cdn.com/-mm-/60c67b4da0efea761b6f18d2a7eb4c3e05ae4c7d/c=241-0-4017-2839&r=x404&c=534x401/local/-/media/2018/04/26/USATODAY/USATODAY/636603455387676668-AP-APTOPIX-Reds-Braves-Baseball-99453123.JPG"
};
```

## Wiring up the presentation

Scroll down to the `teamSelectionChanged` handler. As a reminder, this is the function that is called when the user selects a new team from the `<Picker />` component. In a moment, you'll see we're going to change the binding from a simple string to a member inside of the object, so we're only going to get the `code` in our `value` argument. Since we want our `currentTeam` to actually be the entire object, we'll use Javascript's `find` function to return the right object in the `team` array.

`teamSelectionChanged` will now look like this:

```javascript
teamSelectionChanged = value => {
  this.setState(
    {
      currentTeam: this.teams.find(t => t.code === value)
    },
    () => this.loadStats()
  );
};
```

`value` should be a valid `code`. `find` will return when it finds the first match. Since our codes are unique, this will work perfectly.

## Presentation Rendering

At long last, we've reached the `render()` function. This is where everything is tied together.

The first thing I want to do is generate my `<Picker />` items from the `teams` array. Before the `return` function in `render()`, I want to build a list of `<Picker.Item>`'s.

```javascript
const pickerItems = this.teams.map(team => {
  return <Picker.Item key={team.code} value={team.code} label={team.name} />;
});
```

We'll bind these later on down when we declare our `<Picker />`.

The next set of big changes have to do with using the NativeBase components over the React Native components. By switching some of the layout components to NativeBase the components will play nicer together and map a lot better to the NativeBase examples. Although sometimes I do like to fall back on the standard `<View />` component, the `<Container />` component in NativeBase is usually just as good or better. However, sometimes, it can create some unexpected layout quirks, particularly in flexbox scenarios.

Since this is a massive chunk of code, I'm going to paste it all out there and then we're going to go through each bit and explain what's going on.
{% raw %}

```javascript
return (
  <Container
    style={{
      flex: 1,
      alignContent: "flex-start",
      justifyContent: "flex-start"
    }}
  >
    <Picker
      selectedValue={this.state.currentTeam.code}
      onValueChange={(value, index) => this.teamSelectionChanged(value)}
    >
      {pickerItems}
    </Picker>
    <View>
      {this.state.stats !== null ? (
        <DeckSwiper
          dataSource={this.state.stats}
          renderItem={item => (
            <Card style={{ elevation: 3 }}>
              <CardItem>
                <Left>
                  <Image
                    style={{
                      resizeMode: "contain",
                      width: 80,
                      height: 50
                    }}
                    source={{ uri: this.state.currentTeam.logo }}
                  />
                </Left>
                <Body>
                  <Text style={styles.playerNameText}>
                    {item.firstName} {item.lastName}
                  </Text>
                </Body>
              </CardItem>
              <CardItem cardBody>
                <Image
                  style={{
                    width: 100,
                    height: 100
                  }}
                  source={{ uri: item.image }}
                />
                <View
                  style={{
                    margin: 7,
                    alignSelf: "flex-start",
                    flexDirection: "column"
                  }}
                >
                  <Text style={styles.statText}>
                    Batting Average: {item.battingAverage}
                  </Text>
                  <Text style={styles.statText}>Homeruns: {item.homeruns}</Text>
                </View>
              </CardItem>
              <CardItem>
                <Text>Swipe for Next Player </Text>
                <Icon name="md-arrow-round-forward" />
              </CardItem>
            </Card>
          )}
        />
      ) : null}
    </View>
  </Container>
);
```

{% endraw %}

## Container

The `<Container />` houses our entire component. We've introduced some flexbox stylings. I want our container to take up all the space, I want the content aligned from the top down.

## Picker

Our `<Picker />` has been modified slightly. The `selectedValue` now includes the `.code` at the end since this is now an object. And remember the `pickerItems` variable we declared up top? Well, that gets injected just before our closing `</Picker>` tag. Now we're ready to accept changes to our teams via the picker again, the meat of the presentation is the `<DeckSwiper />` component.

## NativeBase DeckSwiper

The `<DeckSwiper />` component is great for displaying a logical grouping of data that all has the same layout. In our case, we could have a whole lot of sports statistics for a player, but for simplicity sake, we're only showing a few. But if this were the real world, we may have enough to fill a screen and then some. On the Score screen, we elected to have a scrollable list of scores, but for the stats, I'm going to show all of the stats for one player at a time.

The `<DeckSwiper />` is merely a housing for the content. It facilitates the swiping and subsequent rendering and that's it. For each player and the associated stats, we're going to use a `<Card>` which is worthy of its own explanation.

## NativeBase Card

A `<Card>` is another content container, however, it provides us with some flexible options that make it a suitable choice for an array. `<CardItem>` is a child component of `Card` and the two work much the same as `List` and `ListItem`. We're going to take advantage of the `renderItem()` function in `DeckSwiper` and output a `Card` and for each player that we receive back from our API call.

Our `CardItem` is going to utilize the `Left` layout. `Left`, as you might guess, anchors the children inside of it to the left. `<Body>` would place all content between the `Left` and `Right` components and the `Right` anchors the content to the right hand side of the screen.

We're anchoring our current team logo to the left.

{% raw %}

```javascript
<Left>
  <Image
    style={{
      resizeMode: "contain",
      width: 80,
      height: 50
    }}
    source={{ uri: this.state.currentTeam.logo }}
  />
</Left>
```

{% endraw %}

The `<Body>` is going to contain the player name.

```javascript
<Body>
  <Text style={styles.playerNameText}>
    {item.firstName} {item.lastName}
  </Text>
</Body>
```

There is nothing we want to anchor to the right, so we'll omit that component.

Our `cardBody` will display items in a similar fashion, only this time our image will be the hardcoded image of the player and the `<Body>` will contain the player's statistics.

### renderItem()

One notable standout is the `renderItem()` function in the `DeckSwiper`. This is the heart of what ties the data to the presentation layer. We can take the data and template it any way we want to. `renderItem()` takes an array and using ES6 fat arrow syntax, we return jsx for each item. You could literally embed any component(s) you want to render this data. Get comfortable with this way of doing things, it's repeated a lot in various components.

## Wrapping Up

This was a pretty massive set of changes and a lot more polish was possible thanks to NativeBase. NativeBase has a pretty extensive collection of components and I highly recommend trying them out. The nice thing is that they keep to the React Native concepts very well, so what you've learned is transferrable and makes things much more discoverable.

If you hung on to this article, well done! The next article will be a joyride, but NativeBase is making it easy.

## A Note About External Resources (http: vs. https:)

A common stumbling block on images or videos or really anything loaded externally is that iOS (and mostly Android) require that the url be secure. Loading an image from http rather https will result in a frustrating "no load" where you don't get an error, but you also don't get your resource. If you think you have everything right, but nothing is showing, make sure you have https in the url.

## Coming Attractions

You might have noticed that there was a delay displaying the statistics whenever you switch teams. This can be really confusing to a user when the logo reflects one team, but stats are still reflecting the previous selection. In the next article, we'll fix this.

<a href="https://github.com/bbuchanan/react-native-sports-app/tree/b29f09a40258904a0d37975a52ac9c71442209f9" target="_blank">React Native Sports App Tutorial up to this point.</a>

{% include footer.html %}
