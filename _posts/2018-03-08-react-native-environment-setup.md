# Setting Up the React Native Environment

I was first tasked with setting up a simple, proof of concept, "Hello World" application that ran on both iOS and Android.

## Environment Particulars

I'm on a Macbook Pro. I'm using <a href="https://code.visualstudio.com/" target="_blank">Visual Studio Code</a> as my IDE, but I'm not too religious about editors. I've choosen <a href="https://yarnpkg.com" target="_blank">yarn</a> over npm for package management. And of course, git for source control. That's really all you need.

### create-react-native-app

or _crna_ for short. If you're familiar with React, this is more or less equivalent to that. It quickly creates a React Native application with sensible defaults. To get started, I loaded it via yarn.

```
yarn add create-react-native-app global
```

Navigate to the root directory of where you hold your apps/repos. For me, this is my `~/projects` folder.

```
cd ~/projects
create-react-native-app sports-app
cd sports-app
yarn start
```

Out of the box, you can use an app called Expo. Install <a href="https://expo.io/" target="_blank">Expo</a> on your mobile device. Now open your editor and load `App.js`. With Expo running on your device, make some changes to the `<Text>` areas and save them. The application on your phone will update with those changes in near real time. Super awesome!

Congratulations! You now have a functioning mobile application that runs on iOS and Android.

Unfortunately, I have some bad news. All of the goodness we get from _crna_ and Expo is going away almost immediately. I'll explain why and go through the steps next.

## Need a Full Stack React Developer?

I'm your guy! I've been a contract developer for over 14 years and can help you or your company on your project. The best way to contact me is via our company page. [Yye Software](https://www.yyesoftware.com)
