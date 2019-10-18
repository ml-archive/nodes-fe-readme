# Setup your machine

This guide will walk your through the tools you need to install in order to start developing.

## Yarn and Node.js

At Nodes, we use Yarn as our package manager, which is Facebook's version of npm -> it's faster and it uses less space - hurray :tada:.

You’ll need to have Node >= 8 on your local development machine, but don't worry it will be installed automatically together with Yarn.

Use Homebrew to install Yarn ([Need to install Homebrew?](https://brew.sh/))

```console
$ brew install yarn
```

If you are not familiar with Yarn, then have a look at their documentation: [https://yarnpkg.com/en/docs](https://yarnpkg.com/en/docs).

# Node Version Manager (nvm)

It is recommended to install nvm, so you can install multiple versions of node.js, and thereby easily use a specific version for a project:

```console
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.0/install.sh | bash
```

> docs: https://github.com/nvm-sh/nvm

> install guide (bottom of article) https://medium.com/@isaacjoe/best-way-to-install-and-use-nvm-on-mac-e3a3f6bc494d

To install specific version of node:

```console
$ nvm install 6.5.0
$ nvm use 6.5.0
```

To show all node version:

```console
$ nvm ls
```

## Code Editor

To simplify and make it easier to develop React JS, we recommend to use [VS Code](https://code.visualstudio.com/), it has a large library of very useful extensions.

> You are free to use whatever IDE you want, if you can find equivalent plugins/extensions as the mandatory ones but talk with [Themi](https://nodes.slack.com/messages/@thpf) to get it approved.

Install these mandatory extensions:

- ESLint (dbaeumer.vscode-eslint)
- Flow Language Support (flowtype.flow-for-vscode)
- Prettier - Code formatter (esbenp.prettier-vscode)

Install these optional extensions:

- Path Intellisense (christian-kohler.path-intellisense)
- Install both Sass extensions:
  - Sass (robinbentley.sass-indented)
  - SCSS IntelliSense (mrmlnc.vscode-scss)
- TODO Highlight (wayou.vscode-todo-highlight)
- Auto Close Tag (formulahendry.auto-close-tag)
- Auto Complete Tag (formulahendry.auto-complete-tag)
- Auto Rename Tag (formulahendry.auto-rename-tag)
- Color Highlight (naumovs.color-highlight)
- Highlight Matching Tag (vincaslt.highlight-matching-tag)
- JS Refactor (cmstead.jsrefactor)

The project will already be setup up with Workspace settings which will assume that the above extensions are installed.

## Browser

As the main browser for development and testing, we recommend using Google Chrome as it has some very powerful debugging extensions for React and Redux.

Install these extentions:

- [React Developer Tools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi)
- [Redux DevTools](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd)

Before committing your code, you should always check that the new changes work as expected in the browsers:

- Chrome (min. v. 50)
- Firefox (min. v. 40)
- Safari (min. v. 9) (Only if you have Mac)
- Edge (min. v. 12) (Only if you have Windows)

## React Native

You will need Watchman, the React Native command line interface, Xcode, Java Development Kit, and Android Studio.

```console
$ brew install watchman
$ yarn global add react-native-cli
```

To install Xcode, JDK, and Android Studio, follow the rest of the instructions from React Native's [documentation](https://facebook.github.io/react-native/docs/getting-started.html).

![react-native-docs](https://user-images.githubusercontent.com/2675250/41730814-a3b77dbc-7574-11e8-961d-a7538c97ac4c.png)
