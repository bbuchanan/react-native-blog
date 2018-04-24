{% include post-nav.html %}

# Introducing Navigation to our React Native App

Navigation is a bit of a political subject at the moment. I believe the general consensus is that there are pitfalls and cons to whatever you choose. At the time of this writing, there are really 3 choices (several attempts at nav have been abandoned).

* react-native-navigation
* react-navigation
* react-native-navigation v2

Lets start with react-navigation. It's big attractions to me are:

* Simple to get installed. It's Javascript based, so no monkeying around with Android Studio and/or Xcode to get started.
* Community Supported. Commits seem to be frequent and as I'm writing this, there was a commit as recent as 3 days ago.
* Platform specific components.

Cons

* Possible performance issues because it's running on the Javascript thread. For our purposes now, this probably isn't a big issue.
* I'm not a fan of the docs and examples. Could use more screenshots, I found busted links, etc.
* I seem to hit a lot of pitfalls that leave me frustrated.

The folks at reactnavigation.org have even given their own pitch/anti-pitch as to why you should or shouldn't use that library. <a href="https://reactnavigation.org/docs/pitch.html" target="_blank">React navigation pitch/anti-pitch</a>.

### react-native-navigation

Pros

* Native and highly performant
* Seems to be fairly straightforward to get working.
* Documentation is better, I found the samples concise and it got me started quick.

Cons

* Native, so the setup is a pain (as you're probably well aware of after setting up the vector icons).
* Also contains a number of bugs that leave me frustrated.
* Maintained by a company. (wix)
* Deep debugging has to be done in native environments.

### react-native-navigation v2

I cannot speak to this, as I haven't tried it. I wanted to point it out because the folks at wix are encouraging folks to use it. At this stage, it's an alpha product and has been that way for some time and I don't really have a compelling reason to switch to v2. I may play with it some day for fun, but for our purposes, I'm going to eliminate it from the list of consideration.

## My choice is ...

![react-native-navigation](https://cdn-images-1.medium.com/max/1600/1*lPYG2cGAo7Qet0nd6_szPA.png)

React Native Navigation.

As mentioned in the cons, the setup is a pain, but no more painful than the vector icons. It's one and done and the samples provided make up the setup time really quickly.

With that said, I encourage anyone who comes across this today to re-evaluate the react native navigation situation each time you're developing a new app or refactoring an existing app.

{% include footer.html %}
