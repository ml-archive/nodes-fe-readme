# How to test React JS apps

## Frameworks

The testing frameworks used for this project:

- [Jest](https://jestjs.io/docs/en/tutorial-react)
- [Enzyme](https://airbnb.io/enzyme/)
- [jest-enzyme](https://github.com/FormidableLabs/enzyme-matchers/tree/master/packages/jest-enzyme)

Make sure to read them and understand how to use jest and enzyme.

## Running Tests

Command to run tests:

```console
yarn test
```

## Structure

Each component and business logic (folders such as helpers, logic, etc.) should have a folder called `__tests__` where all the tests for that component or logic are defined.

File naming convention:

- `MyClass.js` will have `MyClass.test.js`.

Example of component:

```console
MyClass/
    MyClass.scss
    MyClass.js
    __tests__/
        MyClass.test.js
```

## Writing Tests for Business Logic

It is important to test all the functions that are used to perform calculations or data manipulations, this helps to ensure that the code is robust and any changes will not cause any errors.

Here is a simple example of two tests of a function that calculates the sum of two numbers.

> Only add one `expect` per test, as each test will then only fail for a single reason. Add several tests to cover all test cases.

> Write descriptive test names, so they are easy to locate if they fail.

> Group tests with `describe()` if they are testing the same function. This will provide a better overview of the tests that are related.

```jsx
import sum from "../sum";

describe("sum()", ( => {
  it("sums numbers", () => {
    expect(sum(1, 2)).toEqual(3);
  });

  it("throw error if given wrong parameter", () => {
    expect(sum("not a number", 2)).toThrow();
  });

  it("throw error if missing parameters", () => {
    expect(sum()).toThrow();
  });
})
```

## Writing Tests for Components

All components should be tested, however, if a component is simple, then a smoke test can be enough to check whether the component is rendering or not:

> All components should have this test no matter what!

```jsx
import React from "react";
import { shallow } from "enzyme";
import MyComponent from "../MyComponent";

/** Smoke test */
it("MyComponent: renders without crashing", () => {
  shallow(<MyComponent />);
});
```

For more complex components, we use `jest-enzyme` to write tests that can find rendered elements and perfom asserts on them. For example when we need to ensure that a component renders some DOM elements correctly, has specific states or props.

```jsx
import React from "react";
import { shallow } from "enzyme";
import MyComponent from "../MyComponent";

it("MyComponent: renders welcome message", () => {
  const wrapper = shallow(<MyComponent />);
  const welcome = <h2>Welcome to React</h2>;
  expect(wrapper).toContainReact(welcome);
});

it("MyComponent: allows us to set props", () => {
  const wrapper = mount(<MyComponent title="This is my title" />);
  expect(wrapper.props().title).to.equal("This is my title");
});

it("MyComponent: allows us to change props", () => {
  const wrapper = mount(<MyComponent title="This is my title" />);
  wrapper.setProps({ title: "Another title" });
  expect(wrapper.props().title).to.equal("Another title");
});
```

## Writing Tests for Redux Store

It is important to test the Redux store, as it contains data, functions to manipulate that, and effects that are usually connected to an API.

Several tests can be set up to check if the reducers are working as expected:

```jsx
import { init } from "@rematch/core";
import userModel from "../userModel";

it("reducer: setUser should set a user in the store", () => {
  const store = init({
    models: { userModel }
  });

  const user = { name: "John" }

  store.dispatch.userModel.setUser({ user: user });

  const userModelData = store.getState().userModel;
  expect(userModelData.user).toBe({ name: "John" });
});
```

## Writing Tests for Redux Connected Components

Testing components that are connected to the store can be tricky, but you can mock the store and pass it to the redux connected component as props, which will make it possible to test for some of the functionality:

```jsx
import React from "react";
import { mount } from "enzyme";
import { init } from "@rematch/core";
import userModel from "../userModel";
import Permission from "../Permission";

it("Permission: user has permission to view component", () => {
  const store = init({
    models: { userModel }
  });

  const user = {
    name: "John",
    role: "super-admin"
  }
  const NeedsPermission = <div>NeedsPermission</div>;

  store.dispatch.userModel.setUser({ user: user });
  
  const wrapper = mount(
    <Permission
      requiredPermission="super-access"
      store={store}
    >
      {NeedsPermission}
    </Permission>
  );
  
  expect(wrapper).toContainReact(NeedsPermission);
});
```

### Integration Test of API

To ensure that the API is returning what we expect as well as the function working as expected it can be relevant to perform integration tests. It is then possible to run async calls and await the response from the API, as seen in this example:

```jsx
import { init } from "@rematch/core";
import userModel from './userModel';

it("effect: my getUser should fetch the user from API", async () => {
  const store = init({
    models: { userModel }
  });

  await store.dispatch.userModel.getUser({ userId: 1 });

  const userModelData = store.getState().userModel;
  expect(userModelData.user).toBe({ username: "John" });
});
 ```

More example can be found in the [Rematch Testing](https://github.com/rematch/rematch/blob/master/docs/recipes/testing.md)