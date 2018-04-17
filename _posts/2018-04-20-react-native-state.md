# Component State

Lets do a quick recap of what we have and what we need.

We are loading data, we have written our component view to render that data. We have the data inside of the `componentDidMount` function. We have the scorecard component rendering in the `render()` function. How do we tie these things together?

Most importantly, for now, we need to get the data or anything else from wherever it is loaded, into the `render()` method. For this case, we're going to utilize `state`.

State is available in any class based component. So far, this is what we have. There are stateless or functional components and we'll touch on those later, but for now we have a stateful component so lets set the state.

Go back into the `App.js` file and have a look at the `componentDidMount` function where we have loaded our score data.

```
  componentDidMount() {
    axios.get("scoreboard.json?fordate=20170411").then(data => {
      let scores = data.data.scoreboard.gameScore.map(score => {
        return {
          awayScore: score.awayScore,
          homeScore: score.homeScore,
          homeTeam: `${score.game.homeTeam.City} ${score.game.homeTeam.Name}`,
          awayTeam: `${score.game.awayTeam.City} ${score.game.awayTeam.Name}`,
          location: score.game.location
        };
      });
    });
  }
```

We have declared a local variable named `scores` but the variable is unused. We can create and initialize state at the top of our class. Lets declare a state with a `scores` object and initialize it to `null`.

```
export default class App extends React.Component {
  state = {
    scores: null
  };
```

And now we simply call the `setState()` function upon successful load.

```
componentDidMount() {
  axios.get("scoreboard.json?fordate=20170411").then(data => {
    let scores = data.data.scoreboard.gameScore.map(score => {
      return {
        awayScore: score.awayScore,
        homeScore: score.homeScore,
        homeTeam: `${score.game.homeTeam.City} ${score.game.homeTeam.Name}`,
        awayTeam: `${score.game.awayTeam.City} ${score.game.awayTeam.Name}`,
        location: score.game.location
      };
    });
    this.setState({
      scores
    });
  });
}
```

**_Note_** We used some ES6 shorthand. Since our variable is the same name as the one in the state, we can just say `scores` instead of `scores: scores`.

State variables are accessed throughout the component now by using `this.state`. So in our case, to gain access to the scores, we'd reference `this.state.scores`.

## How about some error handling?

State is also a great way to render an error state. Lets change our state declaration to include an error flag and an error message if we have one.

State initialization would now look like this:

```
state = {
  scores: null,
  hadError: false,
  errorMessage: '',
};
```

and now we could change our `get` logic to include error checking in the `catch` portion of the promise.

```
componentDidMount() {
  axios
    .get("scoreboard.json?fordate=20170411")
    .then(data => {
      let scores = data.data.scoreboard.gameScore.map(score => {
        return {
          awayScore: score.awayScore,
          homeScore: score.homeScore,
          homeTeam: `${score.game.homeTeam.City} ${score.game.homeTeam.Name}`,
          awayTeam: `${score.game.awayTeam.City} ${score.game.awayTeam.Name}`,
          location: score.game.location
        };
      });
      this.setState({
        scores,
        hadError: false,
        errorMessage: ""
      });
    })
    .catch(err => {
      this.setState({
        scores: null,
        hadError: true,
        errorMessage: err.message
      });
    });
}
```

On success (the `then` block), we map the result to our scores and call `setState` where we set `hadError` to false and the `errorMessage` to blank.

On failure (the `catch` block), we set the `scores` to null and flip our error flag to true and set the message return from the http request as our error message.

This is easy enough to test. Remove a character from your URL and you will trigger the `catch` block and your state should reflect that.

## Analyzing State in the Debugger

We can actually prove that the error state was triggered to ourselves by firing up the React Native Debugger. I referenced this tool in a previous post, but briefly, the debugging tool is called React Native Debugger. <a href="https://github.com/jhen0409/react-native-debugger">Installation instructions can be found here.</a>

You can set breakpoints and also use the `debugger;` statement like I did above to pause execution. To stitch your simulator with the debugger requires two orchestrated steps. The first thing you do is start the React Native Debugger application. Next, go into your simulator and hit the hot keys to bring up the React Native. For iOS this is Ctrl+Cmd+Z. For Android it's Ctrl+M. Once in this menu, click Debug JS Remotely.

![enable-debugging](/images/react-native-enable-debugging.png)

But even better for us, we can analyze state as it happens. The only minor pain is that you have to drill down to the component in the Inspector that has the state you want to analyze. Components can get pretty deeply embedded pretty fast.

For me, I removed a digit from the last portion of the url request. So the endpoint address was correct, but I made an invalid request to them. You can see in the debugger that I got an error and that my state was set accordingly.

![react-native-debugger-state](/images/react-native-debugger-state.png)

And on a successful load, we can see the inverse is true. No error and 12 scores.

![react-native-debugger-success](/images/react-native-debugger-success.png)

## Need a Full Stack React Developer?

I'm your guy! I've been a contract developer for over 14 years and can help you or your company on your project. The best way to contact me is via our company page. [Yye Software](https://www.yyesoftware.com)
