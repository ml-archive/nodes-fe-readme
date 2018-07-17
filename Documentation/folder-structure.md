# Folder Structure

This document describes how we at Nodes structure our web project with the aim of creating scalable and maintainable codebases.

## React JS
Almost everything in a React project is a component, but to increase the overview of the project and aid the developers in finding their ways around the code, several folders on different levels have been created. So instead of referring to everything as components, we have this high level abstraction:

app/
- [api/](#api)
- [assets/](#assets)
- [components/](#components)
- [helpers/](#helpers)
- [localization/](#localization)
- [logic/](#logic)
- [partials/](#partials)
- [screens/](#screens)
- [store/](#store)
- [styles/](#styles)
- [types/](#types)
- [utils/](#utils)

Each subfolder has a specific purpose which will be explained in greater detail in the following sections.

### API
This folder contains all the API calls for each feature, such as user, event, article, etc. 

```
api/
    articleApi.js
    eventApi.js
    userApi.js
```

As the number of API calls for each feature increases, subfolders with several files could be created to maintain a good overview like shown below.

```
api/
    article/
        articleGetApi.js
        articlePostApi.js
```

### Assets
Images, fonts and icons that are used in the app should be placed inside this folder.


### Components
This folder contains general components, meaning a component that can be used multiple places throughout the app, such as a button, modal, etc. They are usually configurable, and are used in combination with other general components to create specific components in a screen.
All general components can be used alone or inside other components, and should not have any styling that relies on its parent/environment.

Each component can also consist of several subcomponents, which are either only relevant for the main component or a way to group several components of same type. 

> **Naming**: To allow easy search for files, keep the names short and prefix the subcomponents with the name of the parent.

#### Example: Subcomponent only relevant for parent component
```
Breadcrumb/
    components/
        BreadcrumbItem/
            BreadcrumbItem.js
    Breadcrumb.js
```
Here the `BreadcrumbItem` must only be used in the `Breadcrumb`, simply to reduce complexity of the `Breadcrumb` file. If this structure is respected, then we know that modifying theses subcomponents, eg. `BreadcrumbItem`, will only affect the parent component. In some cases the subcomponents will also have its own set of subcomponents, where the same logic then applies.

#### Example: Subcomponents to group several similar components
```
Icon/
    components/
        IconChevron/
        IconClose/
        ...
    Icon.js
```
Here all the icons in the components folder are exported by the `Icon` component that functions as a [Higher Order Component (HOC)](https://reactjs.org/docs/higher-order-components.html) to reuse as much logic as possible and keep each subcomponent neat and logicless. This means that you will never refer directly to a subcomponent, this also has the advantages that the import path becomes shorter.


### Helpers
Resuable functions that are not part of the business logic should go in here. This could be functions that simply manipulates a string, calculates a number, and so on. Whatever Lodash isn't doing for you, you can put it here.


### Localization
Configurations and locale translation files should go into this folder. [React Intl](https://github.com/yahoo/react-intl) is used for translation of strings, currency and dates/times, however if more complex manipulation of dates/times is necessary then [Moment.js](https://momentjs.com/) is used.


### Logic
The business logic for each features goes into this folder. The goal is keep this folder as small as possible, as the most business logic should be conducted in the backend and not in the frontend. However, many times it can be necessary to manipulate data before sending it to the API, which the logic folder is ideal for.

### Partials
This folder contains components which will be present on all or many screens. Partial components would be a navbar, sidebar or footer, as they often will be positioned absolute or fixed. Therefore they can usually only be added once in the app, as their behaviour is very specific.

### Screens

All the screens for the website will go here. The screens can contain general components and specific components which are only relevant for the respective screen. The following structure resembles a tree structure, where the root is all the main screens (nodes) with links (branches) to their subcomponents and subscreens (nodes).

> **Naming**: All screens should be suffixed with "Screen", to allow easier file search and clarification.

```
HomeScreen/
    components/
        HomeHeader/
        HomeHero/
    screens/
        HomeDetailScreen.js
    HomeScreen.js
```

The screens can have subcomponents, eg. `HomeHeader` and `HomeHero`, these components are specific for this screen and its subscreens, and are not used in any other screens on the same level. This means, that if `HomeHero` was modified, it would only affect `HomeScreen` and potentially the subscreens, which removes the risk of breaking other screens in the rest of the app. 

Each screen can have several subscreens, which are screens only accesible from the parent screen. In this example, the `HomeDetailScreen` could be a screen displayed when a certain item in the `HomeScreen` was clicked. As mentioned before, the `HomeDetailScreen` is allowed to use the subcomponents from the parent or its own subcomponents, if any.


### Store
If the app uses Redux/Rematch, then this folder will hold the store configuration as well as the models, as seen in this example:

```
store/
    models/
        userModel.js
        eventModel.js
    store.js
```


### Styles
Global styling and configurations will be added to this folder. If the project uses Sass, then it will also contain the theme colors, breakpoints and fonts.


### Types
The shared types for [Flow](https://flow.org/) will be added here:
```
types/
    eventType.js
    userType.js
```
Each file will export the attributes for the feature objects, for example:

```jsx
export type UserType = {
    id: string,
    name: string,
    email: string,
    age: number,
    roles: string[]
};
```
This allows Flow to highlight if any attribute types are violated or missing from the type declaration.


### Utils
The global constants and enums are defined in this folder.
