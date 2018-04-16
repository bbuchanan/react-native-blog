# EJECT! EJECT! EJECT

![EJECT](/images/tenor.gif)

The ejection process is necessary for our application for a few reasons, but the immediate reason is that we want to include two native libraries. The first is <a href="https://github.com/oblador/react-native-vector-icons" target="_blank">react-native-vector-icons</a> and the second is <a href="https://github.com/wix/react-native-navigation" target="_blank">react-native-navigation</a>. Ejection essentially allows you to take complete control of the build process. It will allow us to add these native libraries from XCode and Android Studio.

## Post Ejection Pain

If there is one thing I've learned, managing the build and running the software becomes a much more painful process after ejecting. So when you get bogged down with errors (red screen of death), just remember you're not alone. It sucks, there's no way to sugar coat it.

From the command line in the root of your sports-app directory run

```
yarn run eject
```

You'll be asked a few questions, below are my answers.

![react-native-eject-output](/images/react-native-eject.png)

The second question, "What should your app appear as on a user's home screen", I typed in Sports App. For the rest of the questions, I accepted the defaults. It will crank away for a couple of minutes and you're back at a command prompt.

## Setting Up Native Environments

Running the eject command was the easy part. The hard part is getting the native development environments up and running. For iOS, you need XCode. This can be obtained via the App Store. For Android, we're going to use <a href="https://developer.android.com/studio/install.html" target="_blank">Android Studio.</a> The installations will probably take about an hour of your time.

## Building in Native Environments

The eject process has created two new directories off of the app root aptly named ios and android. Respective project files are located within those folders.

## Building and Running in Android Studio

Fire up Android Studio and navigate to the android folder within the project. You'll likely be prompted numerous times for things such as this:

![android-studio-output](/images/android-studio-react-native-1.png)

I usually perform the updates it suggests. The next step is to build. Select `Build->Make Project`. If you're lucky, the build will succeed. If not, see my sympathy section at the end of this article.

The emulator setup is what we're really after though. Navigate to `Tools->AVD Manager`.

![android-studio-virtual-devices](/images/android-studio-avd-virtual-devices.png)

For my purposes, I set up the Pixel 1 (the last one in the list). I'm using API 26 (Oreo). You may have to download that version if you don't have it. After you create your virtual device, have a look at the details. This is done by clicking the down arrow at the far right of your emulator and selecting _View Details_.

![android-studio-virtual-devices](/images/android-studio-avd-emulator-details.png)

Please note the _Name_ with underscores and all. One of the downsides of Android is the emulator must be running when you start the application or it will fail to run. This is different from iOS where the emulator is started for you. For this, I've created a quick command.

```
emulator -avd Pixel_API_26 -dns-server 8.8.8.8 -verbose -no-snapshot
```

Replace the _Pixel_API_26_ with the name in your emulator details. I also specified a DNS address because the emulator wasn't picking up the DNS details from my DHCP server.

The _-no-snapshot_ option is optional but based on previous pain where the emulator or app would misbehave and all too many times clearing the snapshot and/or cold booting were the solutions, so I simply just do that every time.

## Building and Running in XCode

This process seemed a lot simpler than Android Studio. Fire up XCode and navigate to the `~/projects/sports-app/ios` folder and open the xcodeproj.

It's pretty likely the app will not build because of this:

![ios-build-failed](/images/ios-build-failed-1.png)

Select the top level node in your project and select the sportsapp in your targets.

![ios-build-failed](/images/ios-build-failed-2.png)

You'll see the red exclamation mark under the _General_ tab as shown. Click the _Team_ dropdown and select a certificate. Also make sure _Automatically manage signing_ is checked.

After getting things right, your _Signing_ settings should resemble this. Note: you will probably also have to change the _Bundle Identifier_.

![ios-build-failed](/images/ios-build-failed-3.png)

Oh, but it will still fail to build. The reason why is that you also need to make the same change on the sportsappTests target. Fortunately here, you'll just need to pick your "Team" from the dropdown.

Now run `Product->Build` and it should succeed.

## Running Ejected App

`yarn start` will no longer work since Expo cannot be used. We're left with either `yarn run ios` or `yarn run android`. For Android, you must first start the emulator as described above.

If you have successfully built the product in Android Studio and XCode, this part should just work. For most of this, I'll be focusing on iOS, but we'll go back and run Android and tidy things up as well.

So lets give it a go. If this succeeds, you've done fantastic and have lived through half of the pain. I'm going to deliver the other half in the next article when we incorporate the icon and navigation libraries.

## Parting Sympathy

In my limited experience, this is the most difficult part. All the members of my team, including me, have had some hair pulling moments and I'm not always going to know the answers. Your best bet is to google it, but if you have any problems or questions, I'll try to answer/help, but no guarantees.

## Need a Full Stack React Developer?

I'm your guy! I've been a contract developer for over 14 years and can help you or your company on your project. The best way to contact me is via our company page. [Yye Software](https://www.yyesoftware.com)
