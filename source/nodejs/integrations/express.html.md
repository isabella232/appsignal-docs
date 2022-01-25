---
title: "Express"
---

The AppSignal for Node.js integration for Express.js (`express`) v4.0.0+.

## Installation

Add both the `@appsignal/nodejs` and `@appsignal/express` packages to your `package.json`. Then, run `yarn install`/`npm install`.

You can also add these packages to your `package.json` on the command line:

```bash
yarn add @appsignal/nodejs @appsignal/express
npm install --save @appsignal/nodejs @appsignal/express
```

You can then import and use the package in your app.

## Usage

### Middleware

The module includes middleware for automatically instrumenting the routes of your application. The order in which things are imported and used in your application is important, so be sure to note where things are required/imported in the examples below.

!> Don't forget - to auto-instrument modules (including Node.js' internal `http` module, used by `express`), the Appsignal module must be both **required** and **initialized** before any other package.

```js
// AT THE VERY TOP OF THE ENTRYPOINT OF YOUR APPLICATION...
const { Appsignal } = require("@appsignal/nodejs");

const appsignal = new Appsignal({
  active: true,
  name: "<YOUR APPLICATION NAME>",
  pushApiKey: "<YOUR API KEY>", // Note: renamed from `apiKey` in version 2.2.5
});

// ...ALL THE REST OF YOUR IMPORTS AND CODE GO HERE!

const express = require("express");
const { expressMiddleware } = require("@appsignal/express");

const app = express();

// ADD THIS AFTER ANY OTHER EXPRESS MIDDLEWARE, BUT BEFORE ANY ROUTES!
app.use(expressMiddleware(appsignal));
```

### Error Handler

The module also contains a middleware for catching any errors passed to `next()`.

```js
// AT THE VERY TOP OF THE ENTRYPOINT OF YOUR APPLICATION...
const { Appsignal } = require("@appsignal/nodejs");

const appsignal = new Appsignal({
  active: true,
  name: "<YOUR APPLICATION NAME>",
  pushApiKey: "<YOUR API KEY>", // Note: renamed from `apiKey` in version 2.2.5
});

// ...ALL THE REST OF YOUR IMPORTS AND CODE GO HERE!

const express = require("express");
const { expressErrorHandler } = require("@appsignal/express");

const app = express();

// ADD THIS AFTER ANY OTHER EXPRESS MIDDLEWARE, AND AFTER ANY ROUTES!
app.use(expressErrorHandler(appsignal));
```
-> Only exceptions with status code 500 and above will be reported to AppSignal automatically. To send exceptions to AppSignal with other status codes, you can use [custom exception handling](/nodejs/instrumentation/exception-handling.html).
## Example app

An example Express app, containing usage of all of our middleware and custom instrumentation can be found [here](https://github.com/appsignal/appsignal-examples/tree/express).
