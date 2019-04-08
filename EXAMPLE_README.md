# %APP_NAME% - React Native App

This project was set up with following [React Native App Creation Recipe](https://github.com/atolye15/crna-recipe).

## Installation

Run `yarn` to install required packages.

## Configuration

Copy `.env.sample` file as `.env` and change values with as you need. For `.env` usage check [`react-native-config`](https://github.com/luggit/react-native-config) package

**After any change in `.env` file you need to restart react**

## Starting the App

### Bundler

You can run only react-native bundler with the command below:

```bash
yarn start
```

### iOS

You can reference the react native offical documentation
[Running on Simulaor](https://facebook.github.io/react-native/docs/running-on-simulator-ios)

XCode needed to be installed to run app in ios. There is an alias script to run app easily in ios simulator

```bash
yarn run-ios
```

This is alias for `react-native run-ios` you can pass other options as need.

### Android

Android studio to be installed to run app in android. Before run the app, start a virtual device from "Android Virtual Device Manager". When you have running android virtuald device run the following command.

```bash
yarn run-android
```

This is alias for `react-native run-android`. You can pass other options as need.

## Testing

[Jest](https://jestjs.io/) used for testing. To run tests you can use below commands.

To run all test:

```bash
yarn test
```

To run test with watch:

```bash
yarn test:watch
```

To run coverage test:

```bash
yarn coverage
```

> **Minimum 80% test coverage required**

## Storybook

To start storybook run the command below:

```js
yarn storybook
```

Storybook book web ui will be run at <http://0.0.0.0:7007>

To see stories in storybook run a simulator and select a story from web ui to show in device.

## Learn More

You can learn more in the [React Native documentation](https://facebook.github.io/react-native/docs/getting-started).

To learn how to use typescript with React Native check out the [Using TypeScript with React Native](https://facebook.github.io/react-native/blog/2018/05/07/using-typescript-with-react-native).
