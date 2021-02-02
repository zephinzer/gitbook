# JavaScript

## TL;DR

JavaScript is the colloquial name for the ECMAScript language specification and was designed for use in browsers. It has C-styled semantics and is a weak-typed language.

### Why use it

1. You're building a user-facing system
2. Runtimes available for almost all major platforms \(macOS/Windows/Linux/native iOS/native Android/web\)
3. Large availability of entry-level software developers
4. Massive module ecosystem
5. You like C-semantics but dislike types

### Notable OSS projects

1. [Strapi](https://strapi.io/)
2. [Ghost](https://ghost.org/) 
3. [Electron](https://www.electronjs.org/)

## Dependency Management

NPM is used to manage dependencies in JavaScript and comes bundled with a Node.js installation. You can also use Yarn although Yarn and NPM from NPM 6 onwards are pretty much the same. Just be consistent in your use of them across your projects in any product.

Dependencies are listed in a file called `package.json` and are version-locked using `package-lock.json` \(if you're using NPM\) or `yarn.lock` \(if you're using Yarn\). Dependencies are separated into development dependencies and production dependencies which helps to reduce the size of your build artifacts

### Installing existing dependencies

```text
npm install
# OR
yarn
```

### Installing a new development dependency

```text
npm install --save-dev xxx
# OR
yarn add --dev xxx
```

### Installing a new production dependency

```text
npm install xxx
# OR
yarn add xxx
```

## Script Management

## File Management

## Testing

## Building

## Release Management

