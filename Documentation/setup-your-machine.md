# Setup your machine

This guide will walk your through the tools you need to install in order to start developing.

## Yarn and Node.js
At Nodes, we use Yarn as our package manager, which is Facebook's version of npm -> it's faster and it uses less space - hurray :tada:
You’ll need to have Node >= 8 on your local development machine, but don't worry it will be installed automatically together with Yarn.

Use Homebrew to install yarn ([Need to install Homebrew?](https://brew.sh/))

```
brew install yarn
```

If you are not familiar with Yarn, then have a look at their documentation: [https://yarnpkg.com/en/docs](https://yarnpkg.com/en/docs).

## VSCode or PHPStorm
We prefer to use VSCode or PHPStorm, but if you prefer another code editor then talk with [Themi](https://nodes.slack.com/messages/@thpf). 
VSCode can be downloaded for free, but you will need a license for PHPStorm so ask [Jacob](https://nodes.slack.com/messages/@jafr) for that.

I'm preparing an export of settings/plugins for both editors, which will be uploaded here when ready.


## Browser
You will need to install Chrome, Firefox, and Safari.


## Doing React Native?
You will need Watchman, the React Native command line interface, Xcode, Java Development Kit, and Android Studio.

```
brew install watchman
yarn global add react-native-cli
```

To install Xcode, JDK, and Android Studio, follow the rest of the instructions from React Native's [documentation](https://facebook.github.io/react-native/docs/getting-started.html).

![react-native-docs](https://user-images.githubusercontent.com/2675250/41730814-a3b77dbc-7574-11e8-961d-a7538c97ac4c.png)

