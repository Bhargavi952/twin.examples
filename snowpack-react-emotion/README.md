<p><img src="https://i.imgur.com/sVag6YX.png" alt="twin, snowpack, emotion" width="500"></p>

**Download this example using [degit](https://github.com/Rich-Harris/degit)**

```shell
npx degit https://github.com/ben-rogerson/twin.examples/snowpack-react-emotion folder-name
```

From within the new folder, run `npm install`, then `npm start` to start the dev server.

[](#table-of-contents)

## Table of contents

- [Getting started](#getting-started)
  - [Installation](#installation)
  - [Add the global styles](#add-the-global-styles)
  - [Add the twin config (optional)](#add-the-twin-config-optional)
  - [Add the babel config](#add-the-babel-config)
  - [Add the snowpack config](#add-the-snowpack-config)
  - [Add the startup scripts](#add-the-startup-scripts)
- [Customization](#customization)
- [Next steps](#next-steps)

[](#getting-started)

## Getting started

### Installation

Install Snowpack

```shell
npm install --save-dev snowpack @snowpack/app-scripts-react
```

Install the dependencies

```shell
npm install react react-dom @emotion/react @emotion/styled @emotion/babel-plugin-jsx-pragmatic twin.macro tailwindcss
```

<details>
  <summary>Install with Yarn</summary>

Install Snowpack

```shell
yarn add snowpack @snowpack/app-scripts-react --dev
```

Install the dependencies

```shell
yarn add react react-dom @emotion/react @emotion/styled @emotion/babel-plugin-jsx-pragmatic twin.macro tailwindcss
```

</details>

### Add the global styles

Twin uses the same [preflight base styles](https://unpkg.com/tailwindcss/dist/base.css) as Tailwind to smooth over cross-browser inconsistencies.

The `GlobalStyles` import adds these base styles along with some @keyframes for the animation classes and some global css that makes the [ring classes](https://tailwindcss.com/docs/ring-width) and box-shadows work.

You can add Twin’s `GlobalStyles` import in `src/index.js`:

```js
// src/index.js
import React from 'react'
import { render } from 'react-dom'
import { GlobalStyles } from 'twin.macro'
import App from './App'

render(
  <>
    <GlobalStyles />
    <App />
  </>,
  document.getElementById('root'),
)
```

### Add the twin config (optional)

Twin’s config can be added in a couple of different files.

a) Either in `babel-plugin-macros.config.js`:

```js
// babel-plugin-macros.config.js
module.exports = {
  twin: {
    preset: 'emotion',
  },
}
```

b) Or in `package.json`:

```json
// package.json
"babelMacros": {
  "twin": {
    "preset": "emotion"
  }
},
```

Note: The preset gets set to 'emotion' by default, so adding the config is only useful if you want to adjust [Twin’s other options](#twin-options).

### Add the babel config

To use the `tw` and `css` props, emotion must first extend jsx with a [jsx pragma](https://emotion.sh/docs/css-prop#jsx-pragma).

The newest pragma sits at the top of your files and looks like this:

```js
/** @jsxImportSource @emotion/react */
```

But you can avoid adding that pragma yourself with the following babel config:

```js
// babel.config.json
{
  "extends": "@snowpack/app-scripts-react/babel.config.json",
  "plugins": [
    "babel-plugin-macros",
    [
      "@emotion/babel-plugin-jsx-pragmatic",
      {
        "export": "jsx",
        "import": "__cssprop",
        "module": "@emotion/react"
      }
    ],
    [
      "@babel/plugin-transform-react-jsx",
      {
        "pragma": "__cssprop",
        "pragmaFrag": "React.Fragment"
      }
    ]
  ]
}
```

<details>
  <summary>Option #2: Manually specify the jsx pragma in each file</summary>

If you’d rather add the pragma yourself, first add this babel config:

```js
// babel.config.json
{
  "plugins": [
    "extends": "@snowpack/app-scripts-react/babel.config.json",
    "babel-plugin-macros",
    [
      "@babel/plugin-transform-react-jsx",
      {
        "pragma": "__cssprop",
        "pragmaFrag": "React.Fragment"
      }
    ]
  ]
}
```

Then when styling with the tw or css prop, add the pragma at the top of your file. This also replaces the react import:

```js
/** @jsxImportSource @emotion/react */
import tw from 'twin.macro'

const Input = () => <input tw="bg-black" />
// or
const Input = () => <input css={tw`bg-black`} />
```

</details>

> Note: After build, if you’re seeing "process is not defined" then npm install and add `"babel-plugin-transform-inline-environment-variables"` to .babelrc

### Add the snowpack config

Add the following to your snowpack config:

```js
// snowpack.config.js
/** @type {import("snowpack").SnowpackUserConfig } */
module.exports = {
  extends: '@snowpack/app-scripts-react',
  packageOptions: {
    knownEntrypoints: ['@emotion/react', '@emotion/styled'],
  },
}
```

### Add the startup scripts

In `package.json`, add these scripts:

```js
// package.json
"scripts": {
  "start": "snowpack dev",
  "build": "snowpack build"
},
```

[](#customization)

## Customization

- [View the config options →](https://github.com/ben-rogerson/twin.macro/blob/master/docs/options.md)
- [Customizing the tailwind config →](https://github.com/ben-rogerson/twin.macro/blob/master/docs/customizing-config.md)

[](#next-steps)

## Next steps

Learn how to work with twin

- [The prop styling guide](https://github.com/ben-rogerson/twin.macro/blob/master/docs/prop-styling-guide.md) - A must-read guide to level up on prop styling
- [The styled component guide](https://github.com/ben-rogerson/twin.macro/blob/master/docs/styled-component-guide.md) - A must-read guide on getting productive with styled-components

Learn more about emotion

- [Emotion’s css prop](https://emotion.sh/docs/css-prop)
- [Emotion’s css import](https://emotion.sh/docs/css-prop#string-styles)
- [Emotion’s styled import](https://emotion.sh/docs/styled)
