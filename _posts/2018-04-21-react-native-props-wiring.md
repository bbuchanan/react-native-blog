# Wiring up Data via Props

We have written a lot of boilerplate for sure, but hopefully you can see the flexibility and power in this.

Now is the time for us to take the remote data and apply it to our component. <a href="https://reactjs.org/docs/components-and-props.html" target="_blank">React props</a> are a handy way to pass data from parent to child. To go the other way requires a completely different technique using what is called a store. Redux is the package commonly used to handle this scenario. Redux is a huge subject and is out of scope for now.

If you get stuck, <a href="https://github.com/bbuchanan/react-native-sports-app/tree/91fb39024126be0b3ba3809d0b3360ff025f4e3d" target="_blank">the commit up until this point can be found here</a>.

### Props Are Dynamic

Like Javascript itself, props are a nod and a handshake between parent and child components. It takes a tool lke Typescript to enforce some typing. We'll do some Typescript basics later on. For now, we're just going to take the data we've received and render it in our ScorecardItem component.

The first thing we're going to do is modify the ScorecardItem component to render these props.

## Need a Full Stack React Developer?

I'm your guy! I've been a contract developer for over 14 years and can help you or your company on your project. The best way to contact me is via our company page. [Yye Software](https://www.yyesoftware.com)
