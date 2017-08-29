# React Native

## Installation

### For apps without native components

```shell
npm install -g create-react-native-app
```

Create a project

```
cd ~/Projects
mkdir -p react
cd react
create-react-native-app first
cd first
npm start
```

## Running the app

### On your phone

There is a companion app called [Expo](https://expo.io/). Run it in the same wireless network as your computer.

```
cd app
npm start
```

Scan the QR code from the terminal to open your project.


### On a simulator

Make sure you have [Xcode](https://itunes.apple.com/app/xcode/id497799835) installed.  Next, open up Xcode, go to preferences and click the Components tab, install a simulator from the list.

```
npm run ios

# or
# npm start
$ and then press i
```

## Resources

[Blog, React Native Training](https://medium.com/react-native-training)


## FAQ

### ERROR: npm 5 is not supported yet

Yeah. That's fun. There is an [open issue](https://github.com/npm/npm/issues/16991) to track this issue.

The current workarround

```
$ npm --version
5.3.0
$ npm install npm@4 -g
$ npm --version
4.6.1
```

### Project in Expo is not opening

For me that's not working. Don't know why yet. Possibly because I'm in an cafe that's blocking the port.

```
This is taking much longer than it should.
You might want to cancel and check your internet connectivity
```

I left it there and just used the simulator.

### fish: Unknown command 'react-native'

```
npm install -g react-native-cli
```

### Error: ENOENT: no such file or directory, uv_chdir

You don't have an `ios` directory.

That took some googling. My project didn't have an ios (or android directory).
I still don't know how that works. There seems to be two different ways of creating
an initial project but I don't see how you migrate from an expo app to a native app.

```
react-native upgrade
```

Beware this created all the right files, but messed up my `app.json`

### Error: Missing app.json. See https://docs.expo.io/

Well. The file is actually there, so wth.

The file has the wrong structure. See https://github.com/expo/expo/issues/337#issuecomment-313206465

> looks like that was generated with react-native init and not create-react-native-app,
> because it doesn't have an "expo" section. we should provide a better error message in this case.
> app.json in a project created with create-react-native-app looks like this:

```
{
  "expo": {
    "sdkVersion": "18.0.0"
  }
}
```

### Unable to locate device set: Error Domain=NSPOSIXErrorDomain Code=53

Have you recently installed a newer version of Xcode? Possibly a beta version?
You need to switch to the correct version of the tools with `xcode-select`

```
sudo xcode-select -s /Applications/Xcode-beta.app/Contents/Developer
```

### Command failed: /usr/libexec/PlistBuddy

When running `react-native run-ios` I ran into

```
Installing build/Build/Products/Debug-iphonesimulator/arkham-native.app
An error was encountered processing the command (domain=NSPOSIXErrorDomain, code=2):
Failed to install the requested application
An application bundle was not found at the provided path.
Provide a valid path to the desired application bundle.
Print: Entry, ":CFBundleIdentifier", Does Not Exist

Command failed: /usr/libexec/PlistBuddy -c Print:CFBundleIdentifier build/Build/Products/Debug-iphonesimulator/arkham-native.app/Info.plist
Print: Entry, ":CFBundleIdentifier", Does Not Exist
```

What seemed to help was

```
rm -r ~/.rncache
rm -r ios android
react-native eject
```

### App `name` must be defined in the `app.json`

When calling `react-native eject` I got

```
Scanning 718 folders for symlinks in /Users/oliver/Projects/react/arkham-native/node_modules (9ms)
App `name` must be defined in the `app.json` config file to define the project name. It must not contain any spaces or dashes.
```

I'm starting to get really frustrated. Workarounds need other workarounds.
The `app.json` was created by `react-native upgrade` but is created wrong.

According to https://stackoverflow.com/a/42418038 it needs very different fields:

So add `name` (in TitleCase) and `displayName` to the `app.json`

### error: bundling failed: "Cannot find entry file index.ios.js in any of the roots

Now it compiles  but it doesn't run. This is so tiring.

Yeah. Upgrading react seems to [break](https://stackoverflow.com/questions/45594935/expo-io-module-jstimersexecution-is-not-a-registered-callable-module) things.

When I upgraded to a newer react version to solve other issues, I upgraded to a react version not compatible
to expo, so upgrade that

> Make sure to change app.json to the same expo version you have in your package.json. And also make sure you are using the corresponding React-Native version the the expo version you have installed uses. If that doesn't work and your versions match up:

Look up which version of expo is compatible with your react-native version here
https://github.com/react-community/create-react-native-app/blob/master/VERSIONS.md


### Native module  cannot be null

Turns out there are three different ways of developing a react-native app. While they do
mention it in the docs, they don't clearly state the tradeoffs. Or how you, if you forget
that this is the case, like I did, how you even notice.

They strongly recommend to use `reate-react-native-app`, or what they call a CRNA app. This makes use of Expo client, and app that is used as a platform fr your app. If you want other people to use your app, you have to tell them to install the Expo app and import your app. Yeah, that's not going to happen. While maybe nice for quick prototyping, you might program yourself intoa corner by relying on Expo imports.

## Toolkits

[Airbnb, Lottie](https://airbnb.design/lottie/)  renders After Effects animations in real time
