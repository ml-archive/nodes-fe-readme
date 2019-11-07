# Folder Structure

This document describes how we at Nodes structure our web project with the aim of creating scalable and maintainable codebases.

## React JS

Almost everything in a React project is a component, but to increase the overview of the project and aid the developers in finding their ways around the code, several folders on different levels should be created as explained in this document. So instead of referring to everything as components or classes, we have this high level abstraction:

----
app/

- [api/](#api)
- [components/](#components)
- [helpers/](#helpers)
- [layouts/](#layouts)
- [localization/](#localization)
- [logic/](#logic)
- [partials/](#partials)
- [screens/](#screens)
- [store/](#store)
- [storybook/](#storybook)
- [styles/](#styles)
- [types/](#types)
- [configs/](#configs)

public/

----

### APP

Each subfolder above has a specific purpose which will be explained in greater detail in the following sections.

#### API

This folder contains all the API calls for each feature, such as user, event, article, etc.

```console
api/
    articleApi.tsx
    eventApi.tsx
    userApi.tsx
```

As the number of API calls for each feature increases, subfolders with several files could be created to maintain a good overview like shown below.

```console
api/
    article/
        articleGetApi.tsx
        articlePostApi.tsx
```

#### Components

This folder contains general components, meaning a component that can be used multiple places throughout the app, such as a button, modal, etc. They are usually configurable, and are used in combination with other general components to create specific components in a screen. All general components can be used alone or inside other components, and should not have any styling that relies on its parent/environment.

Each component can also consist of several subcomponents, which are either only relevant for the main component or a way to group several components of same type.

> **Naming**: To allow easy search for files, keep the names short and prefix the subcomponents with the full or partial name of the parent.

##### Example: Subcomponent only relevant for parent component

```console
Breadcrumb/
    components/
        BreadcrumbItem/
            BreadcrumbItem.tsx
    Breadcrumb.tsx
```

Here the `BreadcrumbItem` must only be used in the Breadcrumb, simply to reduce complexity of the Breadcrumb file. If this structure is respected, then we know that modifying theses subcomponents, eg. `BreadcrumbItem`, will only affect the parent component. In some cases the subcomponents will also have its own set of subcomponents, where the same logic then applies.

##### Example: Subcomponents to group several similar components

```console
Icon/
    components/
        IconChevron/
        IconClose/
        ...
    Icon.tsx
```

Here all the icons in the components folder are exported by the `Icon` component that functions as a [Higher Order Component (HOC)](https://reactjs.org/docs/higher-order-components.html) to reuse as much logic as possible and keep each subcomponent neat and logicless. This means that you will never refer directly to a subcomponent, this also has the advantages that the import path becomes shorter.

#### Helpers

Resuable functions that are not part of the business logic should go in here. This could be functions that simply manipulates a string, calculates a number, and so on. Before writting any news functions from scratch, make it a habit to check if [Lodash](https://lodash.com/) already does it.

#### Layouts

Some components will have no other purpose than purely specifing the layout of components. For example, a layout with a sidepanel, a header section and a content area is a typical layout that might be shared between several screens. So in order to reuse as much structure and styling as possible, components that are defining the layout should be placed in the `layouts` folder.

#### Localization

Configurations and locale translation files should go into this folder. [React i18next](https://github.com/i18next/react-i18next) is used for translation of text, and for dates and times [Moment.js](https://momentjs.com/) is used.

#### Logic

The business logic for each features goes into this folder. The goal is to keep this folder as small as possible, as most business logic should be conducted in the backend and not in the frontend. However, some times it can be necessary to manipulate data before sending it to the API or storing it in the redux store.

#### Partials

This folder contains components which will be present on all or many screens. Partial components would be a navbar, sidebar or footer, as they often will be positioned absolute or fixed, and only will take up a part of the screen. Therefore they can usually only be added once in the app, as their behaviour is very specific.

#### Screens

All the screens for the website will go here. The screens can contain general components and specific components which are only relevant for the respective screen. The following structure resembles a tree structure, where the root is all the main screens (nodes) with links (branches) to their subcomponents and subscreens (nodes).

> **Naming**: All screens should be suffixed with "Screen", to allow easier file search and clarification.

```console
HomeScreen/
    components/
        HomeHeader/
        HomeHero/
    partials/
        HomeSidebarMenu/
    screens/
        HomeDetailScreen.tsx
    HomeScreen.tsx
```

The screens can have subcomponents, eg. `HomeHeader` and `HomeHero`, these components are specific for this screen and its subscreens, and are not used in any other screens on the same level. This means, that if `HomeHero` was modified, it would only affect `HomeScreen` and potentially the subscreens, which removes the risk of breaking other screens in the rest of the app.

Each screen can have several subscreens, which are screens only accesible from the parent screen. In this example, the `HomeDetailScreen` could be a screen displayed when a certain item in the `HomeScreen` was clicked. As mentioned before, the `HomeDetailScreen` is allowed to use the subcomponents from the parent or its own subcomponents, if any.

#### Store

If the app uses Redux/Rematch, then this folder will hold the store configuration as well as the models, as seen in this example:

```console
store/
    models/
        userModel.tsx
        eventModel.tsx
    store.tsx
```

A model will contain the state, reducers to get and set the data, and async effects to fetch data from the API:

```tsx
export default {
  state: {
    user: {}
  },
  reducers: {
    /**
     * Set User
     * @payload: { user }
     */
    setUser: (state: Object, payload: any) => {
      return {
        ...state,
        user: payload.user
      };
    },
  },
  effects: {
    /**
     * Fetch user from API
     * @payload: { }
     */
    async getUser(payload: Object, rootState: Object) {
      ...
    }
  }
};
```

The `models.tsx` only imports and exports the models, for example:

```tsx
export { default as userModel } from "./userModel";
export { default as eventModel } from "./eventModel";
```

The `store.tsx` then imports the models and specifies which models that should be persisted (offline), for example:

```tsx
import { init } from "@rematch/core";
import * as models from "./models/models";
import createRematchPersist from "@rematch/persist";

const persistPlugin = createRematchPersist({
  whitelist: ["userModel"],
  throttle: 500,
  version: 1
});

const store = init({
  models,
  plugins: [persistPlugin]
});

export default store;
```

Here the `userModel` is added to the whitelist as one of the models that should be persisted in the Redux store for offline usage, which means that if the user refreshes or closes the website, and then goes back to the website then the userModel data will still be there. And, as expected, the rest of the data for the other models will be deleted.

#### Storybook

If the project will have a [storybook](https://github.com/storybooks/storybook), then components that are only used in the stories, such as wrapper components, will be placed in the `storybook` folder.

#### Styles

Global styling and configurations will be added to this folder. If the project uses Sass, then it will also contain the theme colors, breakpoints and fonts. The fonts should be added to a folder under the `styles` folder.

#### Types

The shared types for TypeScript will be added here:

```console
types/
    eventType.tsx
    userType.tsx
```

Each file will export the attributes for the feature objects, for example:

```tsx
export type UserType = {
    id: string,
    name: string,
    email: string,
    age: number,
    roles: string[]
};
```

This will allow VSCode TS to highlight if any attribute types are violated or missing from the type declaration.

#### Configs

The global constants and enums are defined in this folder. For example, an event status could be defined as following:

> Do not add any components or logic in here, only simple files with constants, enums and configs!

```tsx
export enum EventStatus = {
  OPEN: "event-open",
  CLOSED: "event-closed",
  SUSPENDED: "event-suspended"
};
```

### Public

Any code that needs to run immediately like Analytics, meta tags, favicons, etc. can be added directly to the `index.html`.
