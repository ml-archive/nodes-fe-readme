# Setup a new project

Depending on which project you are starting on you can follow these instructions, assuming that you have already read [setup your machine](setup-your-machine.md).

## Framework

### ReactJS

For new ReactJS projects we use [Create React App](https://github.com/facebook/create-react-app) so we have a minimum of configurations.

We will soon create a template with a set of packages and configurations, which can be used for future ReactJS projects.

### VueJS

Projects will be created by following [VueJS documentation](https://vuejs.org/v2/guide/) until a guide has been prepared.

### React Native

New React Native projects will be generated using the react-native-cli:

```console
react-native init NameOfProject
```

> :exclamation: We do **not** use `create-react-native-app` as we will be building, deploying and adding native code in all React Native projects.

We will soon create a template with a set of packages and configurations, which can be used for future React Native projects.

## Github

### Creating the repo

After you've aligned the project name with your project team, you can create the repo on the nodes-projects organization. Check this [guide](https://github.com/nodes-projects/readme/blob/master/general/new-repository.md) (internal) for information on naming of repos.

### License

If you're setting up a customer project, then go ahead and delete the LICENSE file. If it's an open source project, then keep the MIT license file but make sure to update any year references if needed.

### Readme

Have a look at this [template](https://github.com/nodes-projects/readme/blob/master/general/readme-template.md) (internal) for setting up your README file for customer projects.
