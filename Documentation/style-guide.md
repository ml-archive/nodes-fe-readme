# Style Guide

The intention of this document is to guide how we should write JavaScript and CSS/SASS at Nodes. The document should be considered as work-in-progress and PR's with corrections etc. are much appreciated.

## Table of Contents

- [Language](#language)
- [JavaScript Style Guide](#javascript-style-guide)
- [React JS Style Guide](#react-js-style-guide)
- [CSS Modules & SASS](#css-modules--sass)
- [ESLint](#eslint)
- [Flow](#flow)
- [Out-Commented Code](#out-commented-code)
- [Example of React Components](#example-of-simple-react-component)

## Language

Use US English spelling to be consistent with the other dev teams.

## Documentation

Every JS file should always contain inline documentation, we follow a combination of [ESDoc](https://esdoc.org/manual/tags.html) and [JSDoc](http://usejsdoc.org/). This makes it easy for every developer to see what the purpose of the file is. As an example see this simple component:

```jsx
// @flow

import React, { Component } from "react";
import styles from "./Header.module.scss";

type Props = {
  /** Full name of the user. */
  name: string
};

/**
 * A component to show User details
 */
class UserProfile extends Component<Props> {
  render() {
    const { name } = this.props;
    return (
      <div className={styles.container}>
        {name}
      </div>
    )
  }
}

export default UserProfile;
```

## JavaScript Style Guide

We are following the [JavaScript Style Guide by Airbnb](https://github.com/airbnb/javascript).

## React JS Style Guide

To a large extent we are followig [Airbnb's React Style Guide](https://github.com/airbnb/javascript/tree/master/react), however we are doing a few things differently:

- **Extensions**: Use `.js` extension for React components (not `.jsx`).

- **No index**: Do not name any files `index.js` except the root file, as it will make it difficult to perform a search for the file and the tabs in the IDE will say index.js, so it can be difficult to quickly locate which tab you want to edit.

- **Component Naming**:
  - PascalCase for all components
  - camelCase for helper classes, configurations, globals, etc.

## CSS Modules & SASS

For React JS project we use [SASS](https://sass-lang.com/) as it allows us to specify global variables for the theme, such as colors, sizes, animations, etc.

We use [CSS Modules](https://github.com/css-modules/css-modules), so it is not necessary to define unique class names between components as all class names will automatically be made unique when injected into the DOM. However, when creating components, we should still try to be consistent, descriptive, and explicit in the naming so for example:

```jsx
<div className={styles.container}>
  <div className={styles.card}></div>
  <button className={cx(styles.button, { styles.buttonActive: isActive })}></button>
</div>
```

Note, that when we want to have multiple classes we can use [classnames (or cx)](https://github.com/JedWatson/classnames), as well as adding classes conditionally as seen above.

## ESLint

In order to follow the style guidelines, we use ESLint together with `prettier` and `prettier-eslint`, which on save will automatically format your code or highlight any violations you might have made.

## Flow

As JavaScript is a dynamic, weakly typed language, it is prone to errors as they are first revealed at compile time. To mitigate that, we are using [Flow](https://flow.org/), which is a static type checker for JS made by Facebook. It can be added to VSCode and PHPStorm, so it can provide you realtime feedback, thus help us find bugs and errors, before we ship it. In addition, Flow helps when another frontender will move to a new project, as the Component property types and function parameter types will be clearly stated.

Always add to the beginning of a file, so Flow needs which files to analyze:

```jsx
// @flow
```

## Out-Commented Code

Don't leave out-commented code in the project. Or if you feel you really need to, also leave a comment saying why that code is there and why it might be needed again. But in general, delete commented out code. We use git, so you can always get old code from there.

## Example of Simple React Component

This component is stateless and simply just rendering a title. The comments starting with `ONLY FOR EXAMPLE:` is only added for explanation and should not be present in the actual code, where you should only have relevant comments about the implementation.
TODO: Write about why we export named function/class instead of default

```jsx
// @flow

import React, { Component } from "react";
// ONLY FOR EXAMPLE: import the .scss file, not the .css
import styles from "./Header.module.scss";

// ONLY FOR EXAMPLE: define the properties and their types
type Props = {
  /** Title of page. */
  title: string,
  /** The Sub Title of page. */
  subTitle: string
};

/**
 * The Header of a page
 */
// ONLY FOR EXAMPLE: Always use class extends Component
// because then you can easily add internal states if necessary
class Header extends Component<Props> {
  render() {
    // ONLY FOR EXAMPLE: Destructuring the properties
    // or state in the beginning
    // of the render makes it clear which props/states are
    // being used in the layout, and it simplifies the
    // references as there is no need to write
    // `this.props` every time a props/state is used.
    const { title, subTitle } = this.props;
    return (
      <div className={styles.container}>
        <h1>
          {title}
        </h1>
        <h2>
          {subTitle}
        </h2>
      </div>
    )
  }
}

export { Header };
```

## Example of Redux React Component

This component is linked to Redux and updates automatically when the Redux store states are changed.

```jsx
// @flow

import React, { Component } from "react";
import styles from "./UserProfile.module.scss";
import { connect } from "react-redux";

// define the properties and their types
type Props = {
  username: string,
  email: string,
  age: string,
  getUser: Function
};

// Always use class extends Component
// because then you can easily add internal states if necessary
class UserProfile extends Component<Props> {
  // Load the user when the component is mounted
  componentDidMount() {
    this.props.getUser();
  }

  render() {
    // Destructuring the properties or state in the beginning
    // of the render makes it clear which props/states are
    // being used in the layout, and it simplifies the
    // references as there is no need to write
    // `this.props` every time a props/state is used.
    const { username, email, age } = this.props;
    return (
      <div className={styles.container}>
        <div className={styles.text}>
          USERNAME: {username}
        </div>
        <div className={styles.text}>
          EMAIL: {email}
        </div>
        <div className={styles.text}>
          AGE: {age}
        </div>
      </div>
    )
  }
}

// Map the relevant states from the
// userModel in the redux store
const mapState = state => ({
  username: state.userModel.username,
  email: state.userModel.email,
  age: state.userModel.age,
});

// map the functions that we have defined in our userModel
// eg. an api call to fetch a user
const mapDispatch = dispatch => ({
  getUser: () => dispatch.userModel.getUser()
});

// Wrap our component in redux, so the mapping are
// accesible via `this.props`
export default connect(
  mapState,
  mapDispatch
)(UserProfile);
```
