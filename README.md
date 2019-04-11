# React Native App Creation Recipe

This is a step-by-step guide to create React Native app for Atolye15 projects. You can review [React Native App Starter](https://github.com/atolye15/crna-starter) project to see how your application looks like when all steps followed.

You will get an application which has;

* TypeScript
* Linting
* Formatting
* Testing
* CI/CD
* Storybook

## Table of Contents

* [Step 1: Installing the React Native CLI](#step-1-installing-the-react-native-cli)
* [Step 2: Creating a new app](#step-2-creating-a-new-app)
* [Step 3: Make TypeScript more strict](#step-3-make-typescript-more-strict)
* [Step 4: Installing Prettier](#step-4-installing-prettier)
* [Step 5: Installing TSLint](#step-5-installing-tslint)
* [Step 6: Setting up our test environment](#step-6-setting-up-our-test-environment)
* [Step 7: Setting up config variables](#step-7-setting-up-config-variables)
* [Step 8: Organizing Folder Structure](#step-8-organizing-folder-structure)
* [Step 9: Adding Storybook](#step-9-adding-storybook)
* [Step 10: Adding CircleCI config](#step-10-adding-circleci-config)
* [Step 11: Github Settings](#step-11-github-settings)
* [Step 12 Final Touches](#step-12-final-touches)
* [Step 13: Starting to Development <g-emoji class="g-emoji" alias="tada" fallback-src="https://github.githubassets.com/images/icons/emoji/unicode/1f389.png">🎉</g-emoji>](#step-13-starting-to-development-)
* [Bonus: Npm Script Aliases](#bonus-npm-script-aliases)

## Step 1: Installing the React Native CLI

First of all, we need to install the React Native command line interface.

```bash
yarn global add react-native-cli
```

## Step 2: Creating a new app

Use the React Native command line interface to generate a new React Native project called "AwesomeProject":

```bash
react-native init AwesomeProject --template typescript
```

> _NOTE: Project name should be alphanumeric!_

## Step 3: Make TypeScript more strict

We want to keep type safety as strict as possibble. In order to do that, we update `tsconfig.json` with the settings below.

```json
"noImplicitAny": true,
"noImplicitReturns": true,
```

## Step 4: Installing Prettier

We want to format our code automatically. So, we need to install Prettier.

```bash
yarn add prettier --dev
```

```json
// .prettierrc

{
  "printWidth": 100,
  "singleQuote": true,
  "trailingComma": "all"
}
```

Also, we want to enable format on save on VSCode.

> _React Native CLI adds `.vscode` to `.gitignore`, but we prefer not to ignore. So remove it from `.gitignore`._

```json
// .vscode/settings.json

{
  "editor.formatOnSave": true
}
```

Finally, we update `package.json` with related format scripts.

```json
"format": "prettier --write 'src/**/*.{ts,tsx}'",
"format:check": "prettier -c 'src/**/*.{ts,tsx}'"
```

## Step 5: Installing TSLint

We want to have consistency in our codebase and also want to catch mistakes. So, we need to install tslint.

```bash
yarn add tslint tslint-react tslint-config-prettier tslint-consistent-codestyle --dev
```

```json
// tslint.json

{
  "defaultSeverity": "error",
  "extends": ["tslint-react", "tslint-config-prettier", "tslint-consistent-codestyle"],
  "jsRules": {},
  "rules": {},
  "rulesDirectory": [],
  "linterOptions": {}
}
```

We need to update `package.json` for TSLint scripts.

```json
"format": "prettier --write 'src/**/*.{ts,tsx}' && tslint --fix --project .",
"lint": "tsc && tslint --project tsconfig.json"
```

Finally, we need to enable prettier tslint integration on VSCode.

```json
// .vscode/settings.json

{
  // ...
  "prettier.tslintIntegration": true
}
```

## Step 6: Setting up our test environment

We'll use `jest` with `react-native-testing-library`.

```bash
yarn add react-native-testing-library --dev
```

Add the following script into `package.json`

```json
"test": "jest",
"test:watch": "yarn test --watch",
"coverage": "yarn run test --coverage"
```

and then update `jest.config.js` as follows to complete jest configuration.

```js
"collectCoverageFrom": [
  "src/**/*.{ts,tsx}",
  "!src/index.tsx",
  "!src/setupTests.ts",
  "!src/components/**/index.{ts,tsx}",
  "!src/components/**/*.stories.{ts,tsx}"
],
"coverageThreshold": {
  "global": {
    "branches": 80,
    "functions": 80,
    "lines": 80,
    "statements": 80
  }
}
```

Let's add a simple test to verify our setup.

```tsx
// src/App.test.tsx

import 'react-native';
import React from 'react';
import { shallow } from 'react-native-testing-library';

import App from './App';

it('renders correctly', () => {
  const comp = shallow(<App />);

  expect(comp.output).toMatchSnapshot();
});
```

Also, verify coverage report with `yarn coverage`.

When you run `yarn coverage`, a folder named `coverage` will be created in the root directory. This folder is auto-generated file. We should add it to `.gitignore`

```text
# .gitignore

...
# Test Coverage
coverage
```

## Step 7: Setting up config variables

We use the [react-native-config](https://github.com/luggit/react-native-config) package to expose config variables to our javascript code in React Native.

Follow [these steps](https://github.com/luggit/react-native-config#setup) to install.

## Step 8: Organizing Folder Structure

Our folder structure should look like this;

```text
src/
├── App.test.tsx
├── App.tsx
├── __snapshots__
│   └── App.test.tsx.snap
├── components
│   └── Button
│       ├── Button.style.ts
│       ├── Button.stories.tsx
│       ├── Button.test.tsx
│       ├── Button.tsx
│       ├── __snapshots__
│       │   └── Button.test.tsx.snap
│       └── index.ts
├── containers
│   └── Like
│       ├── Like.tsx
│       └── index.ts
├── index.tsx
├── screens
│   ├── Feed
│   │   ├── Feed.style.ts
│   │   ├── Feed.test.tsx
│   │   ├── Feed.tsx
│   │   ├── index.ts
│   │   └── tabs
│   │       ├── Discover
│   │       │   ├── Discover.style.ts
│   │       │   ├── Discover.test.tsx
│   │       │   ├── Discover.tsx
│   │       │   └── index.ts
│   │       └── MostLiked
│   │           ├── MostLiked.test.tsx
│   │           ├── MostLiked.tsx
│   │           └── index.ts
│   ├── Home
│   │   ├── Home.style.ts
│   │   ├── Home.test.tsx
│   │   ├── Home.tsx
│   │   └── index.ts
│   └── index.ts
├── styles
│   ├── Colors.ts
│   ├── Spacing.ts
│   ├── Typography.ts
│   └── index.ts
└── utils
    ├── location.test.ts
    └── location.ts
```

## Step 9: Adding Storybook

We need to initialize the Storybook on our project. We'll use automatic setup with a few edits:

```bash
npx -p @storybook/cli sb init --type react_native
```

> _Warning: Probably after you have run the command above, you'll be asked to select a version. Cancel it._

Storybook CLI automatically installs `v5.0.x`, however `v5.0.x` is an unpublished version for react-native, therefore problems arise during installation. In order to avoid this problem we're going to fix our storybook packages in our `package.json` file to latest stable version `4.1.x`. (Check [this issue](https://github.com/storybooks/storybook/issues/5893) for more information.)

```json
"@storybook/addon-actions": "^4.1.16",
"@storybook/addon-links": "^4.1.16",
"@storybook/addons": "^4.1.16",
"@storybook/react-native": "^4.1.16",
```

thereafter in order to activate the changes and update `yarn.lock` file we'll run code below;

```bash
yarn
```

After completing steps above you'll notice that storybook CLI have created `storybook` folder on your project's root folder. We'll customize this folder structure according to our use case.

Firstly change the name of `index.js` file in `storybook` folder to `storybook.ts`. Also change file extensions of other files from `js` to `ts`.

After that, we create a new file named `index.ts` to expose StorybookUI in your app.

```js
// storybook/index.ts

import StorybookUI from './storybook';

export default StorybookUI;
```

We finished the storybook installation but we are not done yet;

The stories for our app will be inside the `src/components` directory with the `.stories.tsx` extension.The React Native packager resolves all the imports at build-time, so it's not possible to load modules dynamically. we need to use a third party loader [react-native-storybook-loader](https://github.com/elderfo/react-native-storybook-loader) to automatically generate the import statements for all stories.

```bash
yarn add react-native-storybook-loader --dev
```

You need to update `storybook.ts` as follows:

> _Note: Do not forget to replace `%APP_NAME%` with your app name_

```js
// storybook/storybook.ts

import { AppRegistry } from 'react-native';
import { getStorybookUI, configure } from '@storybook/react-native';
import { loadStories } from './storyLoader';

import './rn-addons';

// import stories
configure(() => {
  loadStories();
}, module);

// Refer to https://github.com/storybooks/storybook/tree/master/app/react-native#start-command-parameters
// To find allowed options for getStorybookUI
const StorybookUIRoot = getStorybookUI({});

// If you are using React Native vanilla write your app name here.
// If you use Expo you can safely remove this line.
AppRegistry.registerComponent('%APP_NAME%', () => StorybookUIRoot);

export default StorybookUIRoot;
```

The file `storyLoader.js` that we imported above is an auto-generated file. We should add it to `.gitignore`.

```text
# .gitignore

...
# Storybook
storybook/storyLoader.js
```

Update the storybook script into `package.json` as follows:

```json
"storybook": "watch rnstl ./src --wait=100 | storybook start | yarn start --projectRoot storybook --watchFolders $PWD"
```

Add the following config into `package.json`:

```json
// package.json
{
  "config": {
    "react-native-storybook-loader": {
      "searchDir": ["./src"],
      "pattern": "**/*.stories.tsx",
      "outputFile": "./storybook/storyLoader.js"
    }
  }
}
```

Lastly, because we use typescript in the project, we need to install the type definition for storybook.

```bash
yarn add @types/storybook__react-native --dev
```

Let's create an example story for our Button component.

```tsx
// src/components/Button/Button.stories.tsx

import React from 'react';
import { storiesOf } from '@storybook/react-native';

import Button from './Button';

storiesOf('Button', module)
  .add('Primary', () => <Button theme="primary">Primary Button</Button>)
  .add('Secondary', () => <Button theme="secondary">Secondary Button</Button>);
```

## Step 10: Adding CircleCI config

We can create a CircleCI pipeline in order to CI / CD.

```yaml
# .circleci/config.yml

version: 2
jobs:
  build_dependencies:
    docker:
      - image: circleci/node:10
    working_directory: ~/repo
    steps:
      - checkout
      - attach_workspace:
          at: ~/repo
      - restore_cache:
          keys:
            - dependencies-{{ checksum "package.json" }}
            - dependencies-
      - run:
          name: Install
          command: yarn install
      - save_cache:
          paths:
            - ~/repo/node_modules
          key: dependencies-{{ checksum "package.json" }}
      - persist_to_workspace:
          root: .
          paths: node_modules

  test_app:
    docker:
      - image: circleci/node:10
    working_directory: ~/repo
    steps:
      - checkout
      - attach_workspace:
          at: ~/repo
      - run:
          name: Lint
          command: yarn lint
      - run:
          name: Format
          command: yarn format:check
      - run:
          name: Test
          command: yarn test
      - run:
          name: Coverage
          command: yarn coverage

workflows:
  version: 2
  build_app:
    jobs:
      - build_dependencies
      - test_app:
          requires:
            - build_dependencies
```

After that we need to enable CircleCI for our repository.

## Step 11: Github Settings

We want to protect our `develop` and `master` branches. Also, we want to make sure our test passes and at lest one person reviewed the PR. In order to do that, we need to update branch protection rules like this in GitHub;

![github-branch-settings](https://user-images.githubusercontent.com/1801024/55028896-df2a3300-5019-11e9-9827-a984fcfa29af.png)

## Step 12 Final Touches

We are ready to develop our application. Just a final step, we need to update our `README.md` to explain what we add a script so far.

[EXAMPLE_README](EXAMPLE_README.md)

## Step 13: Starting to Development 🎉

Everything is done! You can start to develop your next awesome React Native application now on 🚀

## Bonus: Npm Script Aliases

### React-Native Alias

```bash
yarn rn
```

`rn` alias for `react-native` allows to run react-native CLI command via locally installed react-native.

```json
// package.json

"rn": "node node_modules/react-native/local-cli/cli.js",
```

### Run On Aliases

```bash
yarn ios
yarn run ios

yarn android
yarn run android

```

`ios` and `android` aliases are helpful when we need to pass different parameter for our project and provides single point entry.

```json
// package.json

"ios": "yarn rn run-ios",
"android": "yarn rn run-android",
```

#### Example

If we want to run our app on iPhone X as default and with scheme just specify that in the alias.

```json
// package.json

"ios": "yarn rn run-ios --simulator 'iPhone X' --scheme 'Production'",
```

## Related

* [cra-recipe](https://github.com/atolye15/cra-recipe/) - CRA Recipe
