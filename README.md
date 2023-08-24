[![dev-tools.ai sdk logo](https://docs.dev-tools.ai/img/logo.svg)](https://dev-tools.ai/)

[![npm version](https://badge.fury.io/js/@devtools-ai%2Fplaywright-sdk.svg)](https://badge.fury.io/js/@devtools-ai%2Fplaywright-sdk)

### Installation

Installation is simple. First add and install the package in your project.

```sh
npm install --save @devtools-ai/playwright-sdk -D
```

```sh
yarn add @devtools-ai/playwright-sdk --dev
```

Then add to your `tests/example.spec.js`

```js
const base = require('@playwright/test');
const devtools = require('@devtools-ai/playwright-sdk');

// Extend basic test by providing a "todoPage" fixture.
const test = base.test.extend({
  page: async ({ page }, use, testInfo) => {
    await devtools.register_devtools(page, testInfo.title);
    await use(page);
  },
});


test('Click on docs link', async ({ page }) => {
  await page.goto('https://playwright.dev/');
  var element = await page.findByAI("docs_link");
  await element.click();
  await new Promise((r) => setTimeout(r, 10000));
}
```

Done! 🎉

### Setting up the plugin

To get started, create a file in your project root folder called `.env`. Then add your API key in it:

```js
DEVTOOLSAI_API_KEY="<<get your_api_key at https://dev-tools.ai>>"
DEVTOOLSAI_INTERACTIVE=FALSE # or TRUE
```

### Interactive Mode

You may also want to enable to interactive mode. Interactive mode pauses the test so you can enter bounding boxes for test elements. These boxes only need to be added once during initial detection. You can enable this mode by exporting this environment variable and setting to true :

```
export DEVTOOLSAI_INTERACTIVE=TRUE
```

or set it in the `.env` file
```js
DEVTOOLSAI_API_KEY="<<your_api_key>>"
DEVTOOLSAI_INTERACTIVE=TRUE
```

### Sample usage

The SmartDriver will automatically kick in once getting an element fails. This applies for the "locator" and "$" Playwright commands.

```js
await page.locator('//some_xpath')
```

There is also a unique command, `findByAI`, that allows you to use human readable names instead of relying on selectors.

```js
await page.findByAI('username-input-field')
```

### Tutorial

We have a detailed step-by-step tutorial to help you get set up with the SDK: https://docs.dev-tools.ai/pw-basic-test-case