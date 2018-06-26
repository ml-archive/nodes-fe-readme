# Style Guide

## Table of Contents
- [Language](#Language)
- [JavaScript Style Guide](#JavaScript-Style-Guide)
- [React JS & React Native Style Guide](#React-JS-&-React-Native-Style-Guide)
- [CSS / SASS](#CSS-/-SASS)
- [ESLint](#ESLint)
- [Flow](#Flow)

## Language

Use US English spelling to be consistent with the other dev teams. 

## JavaScript Style Guide

We are following the [JavaScript Style Guide by Airbnb](https://github.com/airbnb/javascript) .

## React JS & React Native Style Guide

To a large extent we are followig [Airbnb's React Style Guide](https://github.com/airbnb/javascript/tree/master/react), however we are doing a few things differently:

- **Extensions**: Use `.js` extension for React components (not `.jsx`).

- **No index**: Do not name any files `index.js` except the root file, as it will make it very difficult to perform a search for the file.

- **Component Naming**: Use PascalCase for all components, but for helper classes, configuations, globals, etc. use camelCase.

## CSS / SASS
For React JS project we use [SASS](https://sass-lang.com/) as it allows us to specify global variables for the theme, such as colors, sizes, animations, etc.

We use the naming convention BEM (Block-Element-Modifier, docs: [bem.info](https://en.bem.info/methodology/quick-start/) & [getbem.com](http://getbem.com/naming/)) for classes in HTML and CSS, in order to be consistent in the naming and to mitigate overlapping styles.

```html
<div class="block">
  <div class="block__element"></div>
  <div class="block__element block__element--active"></div>
</div>
```

```scss
.block {
   color: white;
 }
.block__element { 
  background-color: black;

  &--active {
    background-color: green;
  }
}
```
Each block, should have a unique and descriptive name, so there will be not be any clas styles that overwrites each other. A good rule is to name the block the same as the component, for example if the component is called `NavigationBar`, then the block class name should be `<div class="navigation-bar">`.

Elements can have more general names, as they are nested in the block, and therefore prefixed with a unique name.

## ESLint

In order to follow the guidelines, we use ESLint together with the `prettier` and `prettier-eslint`, which on save will automatically format your code or highlight any violations you might have made.

A configuration file for ESLint will be made soon.

## Flow

As JavaScript is a dynamic, weakly typed language, it is prone to errors as they are first revealed at compile time. To mitigate that, we are using [Flow](https://flow.org/), which is a static type checker for JS made by Facebook. It can be added to VSCode and PHPStorm, so it can provide you realtime feedback, thus help us find bugs and errors, before we ship it. In addition, Flow helps when another frontender will move to a new project, as the Component property types and function parameter types will be clearly stated.


## Out-Commented Code
Don't leave out-commented code. Or if you feel you really need to, also leave a comment saying why that code is there and why it might be needed again. But in general, delete commented out code. We use git, so you can always get old code from there.
