{% include post-nav.html %}

# Error Reporting Using Toast Component

In this article, we're going to throw in some quick error reporting to the user. This will occur when the `catch()` block is executed due to one or more errors. Since we already have a vast majority of the building blocks in place, this should be fairly straightforward.

The first thing to note is that React Native does not have a toast component. In fact, out of the box react native components are a bit sparse, so we'll find ourselves going to third parties more often than a reactjs app.

I've tried a few toast components, with mixed success. I've settled on one that's simple enough for our purposes. It's called `react-native-root-toast`. To get started, install the component:

`yarn add react-native-root-toast`

Strangely, there is a dependency that isn't installed by default called `redux`. Redux is an extremely popular library that you have probably stumbled across. If not, don't worry about it for now, it's nothing more than a depedency for us at this stage. We'll go into redux at a later time as it is more of an advanced topic. Back to the task at hand, install redux.

`yarn add redux`

### Wiring Up Your Toast Component

We have everything we need to simply wire this up. First, import the toast component. At the top of App.js probably below where you imported the DatePicker component, add:

`import Toast from "react-native-root-toast";`

Then at the top of the `render()` function, we'll conditionally create the toast component if there is an error set in our state.

```javascript
let toast = null;
if (this.state.hadError) {
  toast = <Toast visible>{this.state.errorMessage}</Toast>;
}
```

Then at nearly the bottom of the `return` statement in the render function, we'll add our toast variable just before we close off the `</View>` tag.

```javascript
  {toast}
</View>
```

We could inline this if we wanted to, but I think my render function is getting big enough.

Finally, lets create an error condition in the request so that it will fail and display the toast component. I've added a bogus 1 to the beginning of the query string in the url.

```javascript
axios.get(`scoreboard.json?1fordate=${this.state.date}`);
```

![react-native-toast-notification]({{ site.imageurl }}/react-native-toast-notification.png)

As usual, if you're stuck you can find my commit for this <a href="https://github.com/bbuchanan/react-native-sports-app/tree/4de613eab6d94aee74f3ea9b8f407decb1586c4e" target="_blank">react native tutorial
here</a>.

{% include footer.html %}
