{% include post-nav.html %}

## Lets Write Some Code

Finally we're in a position to write some simple code. A picture is worth a thousand words, so a video must be worth much more. Below is all we're going to accomplish with this post, but it's kind of a big deal.

<video width="640" height="480" controls>
  <source src="{{ site.videourl }}/react-native-hello-world.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>

We're getting exposure to simple tags. React Native is not React and does not use HTML, but the concepts are similar. To display text, we use the `<Text />` tag. For a complete list of components, see.

https://facebook.github.io/react-native/docs/components-and-apis.html

Some of the more common tags that we'll be usings are:

* View
* Image
* FlatList

The list of components is relatively small compared to reactjs, so later on we'll be utilizing another external library called <a href="https://nativebase.io/" target="_blank">NativeBase</a>.

Back to our tiny React Native app. The video shows us removing the default boilerplate and putting a single `<Text>` tag in with the words Hello World. Easy enough.

## Container Components

If you're familiar with reactjs, you know that you need a container component to house things like text and image components. In React Native, the simplest form is the `<View />` component.

Later on we'll deal with more robust containers that allow us to scroll, layout and refresh.

## React Native Styling

React Native is not HTML. The styling tags look remarkably similar to CSS, but it is not CSS. A great resource for React Native styling tags that are available is the <a href="https://github.com/vhpoet/react-native-styling-cheat-sheet" target="_blank">React Native Cheatsheet</a>.

## Layout with Flexbox

React Native uses flexbox to arrange items within a container. This is functionally similar to CSS flexbox. It's beyond the scope of this tutorial to teach all of the concepts of flexbox, but we'll go over some basics in the next tutorial and touch on particulars when they're relevant to the application that we're building.

{% include footer.html %}
