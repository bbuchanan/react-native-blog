# Making Remote Requests with Axios

Slowly we're getting into the meat of the project. The goal is to get details of sports from a remote source. In this case, we're going to use <a href="https://www.mysportsfeeds.com" target="_blank">My Sports Data Feed</a>. The API is free for completed seasons. So we're going to focus on the 2017 baseball season, but this can easily be changed to suit your sport of choice.

You will need to sign up for a free account, as each request requires your user name and password. You can see mine here, but I'm changing the password before this is published so it won't work with my credentials.
<a href="https://github.com/bbuchanan/react-native-sports-app/commit/934320ea5512a2d123ac103c0d4bdc4d8b2a423b" target="_blank">Oh and here is the source code to reference should you get stuck or something isn't clear. </a>

### Installing Axios

Axios is a library we're going to use to make http calls to My Sports Feed. There is a "built-in" library named `fetch`, but I prefer axios for a couple of reasons. First, `fetch()` has a two-step process when handing JSON data. The first is to make the actual request and then the second is to call the `.json()` method on the response. Second, I like the configurability of axios, specifically the interceptors and having a global configuration that removes a lot of the duplication and verbosity. Topics for fodder some other time.

Back to business. To install `axios` run `yarn add axios` from your terminal. `axios` is promise based, so we'll be able to chain our processing code.

### Configuring Axios

As mentioned above, I like the flexibility and ease of use of axios. I'm going to take advantage of that right away by extending the axios instance like so.

First, in the project, I'm going to create a new folder off of the root called 'src'. Inside of that folder we'll create a file named `axios-sports.js`.

![axios-sports](/images/axios-sports.png)

Inside of this file, I'm going to set some defaults that will save us from duplicating code and url details.

```javascript
import axios from "axios";

const instance = axios.create({
  baseURL: "https://api.mysportsfeeds.com/v1.2/pull/mlb/2017-regular/"
});

instance.defaults.headers.common["authorization"] =
  "Basic YmlsbGIyMTEyOnl5ZXl5ZSQx";

export default instance;
```

The baseURL is the 2017 Major League Baseball season. We're going to simply pull score information.

The `authorization` bit will need to reflect your user account details. These are mine, but they're no longer valid, so please get your own in there. The format is `userid:password` encoded in base64. <a href="https://www.base64encode.org/" target="_blank">Here is a good place to base64 encode a string.</a> Replace my string with the output of this after the word "Basic ".

That's probably the hardest part about getting this set up.

### GETting data

One thing we haven't touched on yet is component life cycle events in React. This is a noteworthy topic, but we're going to skip it for now. Just know that they exist and know they're special methods. We're going to use one now to get our data.

`componentDidMount` is an event generated when the react component is mounted and ready. This is a good time to get data and that's precisely what we're going to do.

Go back into `App.js` and import axios.

`import axios from "./src/axios-sports";`

Inside of the class, add a hook for `componentDidMount` like so:

```javascript
  componentDidMount() {
    axios.get("scoreboard.json?fordate=20170411").then(data => {
      debugger;
    });
  }
```

We're getting the Major League scores for April 11, 2017 in json format. Assuming our promise is successful, the result is returned in `data`.

We're not doing anything with the data as of yet, but look at how simple and clean it is to retrieve remote data and act on it.

### Debugging Sidebar

For starters, we simply want to ensure that all of the plumbing is in place and grabbing data as expected, so the user interface does nothing at this point, but I'd like to inspect the data coming back.

React Native has a pretty damn good debugging tool called React Native Debugger. <a href="https://github.com/jhen0409/react-native-debugger">Installation instructions can be found here.</a>

You can set breakpoints and also use the `debugger;` statement like I did above to pause execution. To stitch your simulator with the debugger requires two orchestrated steps. The first thing you do is start the React Native Debugger application. Next, go into your simulator and hit the hot keys to bring up the React Native. For iOS this is Ctrl+Cmd+Z. For Android it's Ctrl+M. Once in this menu, click Debug JS Remotely.

![enable-debugging](/images/react-native-enable-debugging.png)

There are more hot keys for auto refreshing and some other neat tricks that can be found on the <a href="https://facebook.github.io/react-native/docs/debugging.html" target="_blank">official debugging section</a> in the documentation.

I highly encourage you to set this up now and examine the output with me.

### Examining GET Results

With your debugger loaded, Reload the application and when it starts, you should hit the breakpoint on success.

![react-native-debugger-breakpoint](/images/react-native-debugger-breakpoint.png)

Once stopped at a breakpoint, we can examine the details of the request in a number of ways. For now, I'm simply going to type `data` into the Console window and expand the relevant parts.

![react-native-debug-output](/images/react-native-debug-output.png)

The most interesting parts here are the `status` property and the `data` property. Status code 200 indicates a successful call which means the `data` property will be filled with whatever we requested. In this case, it's the score data for April 11, 2017.

Now that we're getting data back from our service, we can finally focus on parsing the data and making it pretty.

## Need a Full Stack React Developer?

I'm your guy! I've been a contract developer for over 14 years and can help you or your company on your project. The best way to contact me is via our company page. [Yye Software](https://www.yyesoftware.com)
