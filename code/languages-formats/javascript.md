# JavaScript

## TL;DR

JavaScript is the colloquial name for the ECMAScript language specification and was designed for use in browsers. It has C-styled semantics and is a weak-typed language.

### Why use it

1. You're building a user-facing system
2. Runtimes available for almost all major platforms (macOS/Windows/Linux/native iOS/native Android/web)
3. Large availability of entry-level software developers
4. Massive module ecosystem
5. You like C-semantics but dislike types

### Notable OSS projects

1. [Strapi](https://strapi.io)
2. [Ghost](https://ghost.org) 
3. [Electron](https://www.electronjs.org)

## Dependency Management

NPM is used to manage dependencies in JavaScript and comes bundled with a Node.js installation. You can also use Yarn although Yarn and NPM from NPM 6 onwards are pretty much the same. Just be consistent in your use of them across your projects in any product.

Dependencies are listed in a file called `package.json` and are version-locked using `package-lock.json` (if you're using NPM) or `yarn.lock` (if you're using Yarn). Dependencies are separated into development dependencies and production dependencies which helps to reduce the size of your build artifacts

### Installing existing dependencies

```
npm install
# OR
yarn
```

### Installing a new development dependency

```
npm install --save-dev xxx
# OR
yarn add --dev xxx
```

### Installing a new production dependency

```
npm install xxx
# OR
yarn add xxx
```

## Script Management

Project specific scripts are typically stored in the `package.json` file in the `"scripts"` root-level property. An example of such a `package.json` with `"scripts"` follows:

```javascript
{
  "name": "packagename",
  "version": "1.0.0",
  "description": "lorem ipsum",
  "main": "index.js",
  "scripts": {
    "start": "echo 'start'"
  },
  "keywords": [],
  "author": "zephinzer",
  "license": "MIT"
}
```

Common scripts you might find in a `package.json` file and their usual intentions are as follows:

1. **`start`**: starts the application in development mode
2. **`test`**: runs the unit tests for the application

You can also add a `pre` prefix to any of the scripts to indicate that another command should be run before the target script. For example, a script named `prestart` will run before `start`.

To execute scripts, simply execute `npm run $SCRIPT_NAME`.

Refer to [the `npm` documentation at this link](https://docs.npmjs.com/cli/v7/using-npm/scripts) for the full documentation.

## File Management

Javascript is a wild, wild place and in general there are hardly any conventions you will find. So here are some conventions I've found useful to follow:

### Front-end projects

Front-end projects are typically code destined for deployment as websites. Think React, Svelte, Angular, or Vue. Or even plain vanilla HTML. The code is typically built by a bundler such as `rollup` or `webpack` or transpiled by `tsc` if it is in Typescript. The result is usually a single directory containing HTML,CSS, and JS files that can be deployed as a static website and is found at `./build`.

For such projects, I've found it useful to place all source code into a `./src` directory relative to the project root. Within this `./src` directory, structure the directories based on the path in the website and link to these from a `routes.js` file

```
/project
  /build             # contains the 
  /src               # contains the source code
    /path            # 
      /in            #
        /Webapp      # /path/in/webapp should load this
          /index.js  # file to make requiring easier
          /Webapp.js # file containing source code
    /other           #
      /Path          # /other/path should load this
        /index.js    # file to make requiring easier
        /Path.js     # file containg source code
    /utils           # shared components
    /App.js          # application entrypoint
    /routes.js       # contains declarative routing rules
    /index.js        # website entrypoint (should load App.js)
```

Files containing component code should be in `CapitalCase`, files containing controller code should be in `lower-kebab-case`.

For each component, include an `index.js` that imports the component and exports it as the default. This will let you `require` dependencies with `const dep = require('../path/to/Component')` rather than `const dep = require('../path/to/Component/Component')`. In the `index.js`, you can also handle other glue code necessary for events like loading/verifying state so that the component code itself is clean of state. This improves testability because the component can be tested in isolation without state, and the state loaders can be tested using a mock component.

### Back-end projects

Back-end projects are typically code destined for deployment as a non-user facing service. The code is not typically built unless the source is in Typescript, in which case the code is transpiled before deployment.

A project structure I've found useful

## Testing

## Building

## Release Management
