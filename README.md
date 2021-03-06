# jest-nightwatch-vue

<p align=center>
  <img alt="Nightwatch.js Schematic Logo" src=".github/assets/nightwatch-logo.svg" width=200 />
  <img alt="Jest Logo" src=".github/assets/jest-logo.png" width=200 />
</p>

[![Build Status][build-badge]][build]

Boilerplate project which integrates Nightwatch with [Jest](https://jestjs.io/) for testing Vue 3 components.
To render the components, the [Vite](https://vitejs.dev/) dev server is used.  

## Getting Started

### Nightwatch Jest Environment
In order to use Nightwatch in Jest, you need to install the **[`jest-environment-nightwatch`](https://github.com/nightwatchjs/jest-environment-nightwatch/)**.


Install Jest, Nightwatch, and other dependencies needed for testing. 

```sh
npm i jest nightwatch jest-environment-nightwatch --save-dev
```

### Webdriver

Nightwatch needs a [W3C Webdriver](https://www.w3.org/TR/webdriver/) compatible service, depending on which browser you'll be using.

### For Chrome:
Install the `chromedriver` package from NPM with:

```sh
npm i chromedriver --save-dev
```

### For Firefox:
Install the `geckodriver` package from NPM with:

```sh
npm i geckodriver --save-dev
```

## jest.config.js
Update your Jest config file and add the Nightwatch environment.

```js
module.exports = {
  testEnvironment: 'jest-environment-nightwatch'
}
```

Head over to the [`jest-environment-nightwatch`](https://github.com/nightwatchjs/jest-environment-nightwatch) repo to see usage and configuration. The Nightwatch Jest environment comes with sensible defaults, but you will probably need to update the config.

Here's an example used in this project:

```js
module.exports = {
  testEnvironment: 'jest-environment-nightwatch',
  testEnvironmentOptions: {
    capabilities: {},
    env: 'chrome',
    headless: true,
    devtools: false,
    debug: false,
    output: false,
    silent: false,
    parallel: true,
    timeout: 500,
    baseUrl: 'http://localhost:3005',
    async setup(browser) {
      if (global.viteServerUrl) {
        browser.baseUrl = global.viteServerUrl;
      }
    }
  }
}
```

## Component Testing
This project is setup for component testing, which is done using the [Vite](https://vitejs.dev/) dev server and the Nightwatch plugin for Vite: [`vite-plugin-nightwatch`](https://github.com/nightwatchjs/vite-plugin-nightwatch/).

In order to run the component tests, two things are needed:

### Install Vite and plugins
```sh
npm i vite vite-plugin-nightwatch @vitejs/plugin-vue --save-dev
```

You'll also need to install the `vuex` and `vue-router` to run the tests:
```sh
npm i vuex@next vue-router@next --save
```

### Updated jest.config.js
Add the `globalSetup` and `globalTeardown` files located in the `lib` folder in order to start/stop the Vite dev server automatically.

```js
module.exports = {
  globalSetup: './lib/setup.js',
  globalTeardown: './lib/teardown.js'
}
```

### Run the tests
This will run the included example tests in the `test` folder:

- `formComponentTest.js` - tests a basic Form component
- `vuexTest.js` - example on how to test a component which uses a [Vuex](https://vuex.vuejs.org/) store
- `vueRouterTest.js` - example on how to test a component using the [VueStore](https://router.vuejs.org/)

```js
npm test
```

Jest runs the tests in parallel by default. Here's an example output:

![terminal_screenshot](https://user-images.githubusercontent.com/419506/153366485-2a90f030-3b62-4c31-9b0d-27f5f8dfbcfa.png)

[build-badge]: https://github.com/nightwatchjs/jest-nightwatch-vue/actions/workflows/node.js.yml/badge.svg?branch=main
[build]: https://github.com/nightwatchjs/jest-nightwatch-vue/actions/workflows/node.js.yml

