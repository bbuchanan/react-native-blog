{% include post-nav.html %}

# Responding to User Feedback - React Native Events

After that bit of clean up, I'd like to allow the user to pick dates for scores they'd like to see. We're going to go through installing a third party component package and show how to handle user events.

Here is the goal of this section.

<video width="640" height="480" controls>
  <source src="{{ site.videourl }}/react-native-date-picker.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>

React Native provides separate components for date picking in iOS and date picking on Android which is not ideal. For this, I'm electing to download a package that handles both platforms with a single control.

Run

`yarn add react-native-datepicker`

to get this component installed.

To begin using this component, we'll need to `import` it. At the upper portion where the other import statements live add a new line.

`import DatePicker from "react-native-datepicker";`

We're going to hold the selected date in our component's state, so in your state initializer, add the following:

`date: "20170411`

which is what our current hard coded date value is.

Now we're going to add the DatePicker to the top of our `<View>`, just above the FlatList. I've taken their example and tweaked the props to suit my purposes.

{% raw %}

```javascript
<DatePicker
  style={{ width: 200 }}
  date={this.state.date}
  mode="date"
  placeholder="select date"
  format="YYYY-MM-DD"
  minDate="2016-05-01"
  maxDate="2017-11-01"
  confirmBtnText="Confirm"
  cancelBtnText="Cancel"
  onDateChange={date => this.dateChangedHandler(date)}
/>
```

{% endraw %}
Also note that I had to omit the `customStyles` property from this tutorial because github pages didn't like it. You can copy the `<DatePicker>` props in full from <a href="https://github.com/bbuchanan/react-native-sports-app/blob/5779968579d84006d6fb48148c1e0e076c1528a3/App.js" target="_blank">here</a>.

The vast majority of these props seem pretty self explanatory, but I do want to touch on the event that we're handling, since this is a new concept. `onDateChange` is a property that takes a function. The function is called when the user confirms the picked date or time in the UI. There is some shorthand going on here. The fat arrow says to pass the function as opposed to executing it immediately. The `date` declaration is an argument passed in by the `DatePicker` and we're in turn passing this information on to a function we named `dateChangedHandler`.

Before we get to the `dateChangedHandler` function, I wanted to point out another refactor that has been done. I've moved the entire contents of `componentDidMount` into a separate function named `loadScores`. `loadScores` takes no arguments because we're going to get what we need from the state. Create a function like this:

```javascript
loadScores = () => {};
```

and take everything from `componentDidMount` and put it in here. After that's done and your `componentDidMount` function is empty, add this single line to `componentDidMount`: `this.loadScores();`. So now we're still going to have some scores loaded when the app starts, but now the function can be called from other events as well.

Oh, one other thing, make sure you change the http get call from the hard coded value to the date that's in our state.

`` .get(`scoreboard.json?fordate=${this.state.date}`) ``

This takes us back to our `dateChangedHandler` function that we're calling when the selection on the `DatePicker` changes, but have yet to write. I want to accomplish two things. I need to update the state with the new date and I want the scores to reload reflecting the date selected.

The `DatePicker` formats the date in a visually appealing manner, however, our API requires the date in `YYYYMMDD`, so we're going to use `moment.js` to take the visually appealing version and make it suitable for the sports API.

`Moment.js` likely already installed, but if it isn't `yarn add moment` will get that straight. We'll want to import it at the top of our file as well. Where you added the `DatePicker` import, add `import moment from "moment";`.

Finally, we can get to writing our `dateChangedHandler` event handler function.

```javascript
dateChangedHandler = date => {
  this.setState({ date: moment(date).format("YYYYMMDD") });
  this.loadScores();
};
```

It's as simple as I explained earlier. We're setting the state and using moment to format the date appropriately and then calling `this.loadScores()`.

As usual, if you're stuck you can find my commit for this <a href="https://github.com/bbuchanan/react-native-sports-app/blob/5779968579d84006d6fb48148c1e0e076c1528a3/App.js" target="_blank">react native tutorial
here</a>.

Compare my code against yours to see where we're different.

{% include footer.html %}
