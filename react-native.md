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

### fish: Unknown command 'react-native'

```
npm install -g react-native-cli
```

