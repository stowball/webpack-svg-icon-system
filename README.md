# webpack-svg-icon-system

[![npm version](https://badge.fury.io/js/webpack-svg-icon-system.svg)](https://badge.fury.io/js/webpack-svg-icon-system)
[![Build Status](https://travis-ci.org/stowball/webpack-svg-icon-system.svg?branch=master)](https://travis-ci.org/stowball/webpack-svg-icon-system)

A loader and plugin for webpack that converts all your SVGs into symbols and merges them into a SVG sprite.

This is a simpler fork of [Karify's external-svg-sprite-loader](https://github.com/Karify/external-svg-sprite-loader).

## Requirements

You will need NodeJS v6+, npm v3+ and webpack 2.

To make it work in older browsers, like Internet Explorer, you will also need [SVG for Everybody](https://github.com/jonathantneal/svg4everybody) or [svgxuse](https://github.com/Keyamoon/svgxuse).

## Installation

```bash
npm i webpack-svg-icon-system
```

## Options

### Loader options

- `name` - relative path to the sprite file (default: `img/sprite.svg`). The `[hash]` placeholder is supported.
- `prefix` - value to be prefixed to the icon's name (default: `icon`).
- `suffix` - value to be suffixed to the icon's name (default: `''`). The `[hash]` placeholder is supported.
- `svgoOptions` - custom options to be passed to svgo.

### Plugin options

- `emit` - determines if the sprite is supposed to be emitted (default: true). Useful when generating server rendering bundles where you just need the SVG sprite URLs but not the sprite itself.

## Usage

If you have the following webpack configuration:

```js
// webpack.config.js

import path from 'path';
import SvgStorePlugin from 'webpack-svg-icon-system/lib/SvgStorePlugin';

module.exports = {
    module: {
        rules: [
            {
                loader: 'webpack-svg-icon-system',
                test: /\.svg$/,
                options: {
                  // override default options
                },
            },
        ],
    },
    output: {
        path: path.resolve(__dirname, 'public'),
        publicPath: '/',
    },
    plugins: [
        new SvgStorePlugin(),
    ],
};
```

You will be able to import your SVG files in your JavaScript files as shown below.
The imported SVG will always correspond to a JavaScript object with keys `symbol`, and `viewBox`:
- The `symbol` url can be used on a `<use>` tag to display the icon;
- The `viewBox` value is required by some browsers on the `<svg>` tag.

The URLs will have the following format:
- `symbol`: `webpackConfig.output.publicPath`/`loader.name`#`loader.prefix`-`your-svg-file-name`-`icon-file-hash`

### Add your sprite to the DOM

```js
(function(path, baseUrl) {
  var id = 'svg';
  var xhr = new XMLHttpRequest;
  var body = document.body;
  var div = document.createElement('div');
  var base = baseUrl || window.location.protocol + '//' + window.location.hostname + (window.location.port ? ':' + window.location.port : '');
  var url = base + path;

  if (div.id = id, body.insertBefore(div, body.childNodes[0]), 'withCredentials' in xhr) {
    xhr.withCredentials;
    xhr.open('GET', url, true);
  }
  else {
    if (typeof XDomainRequest == 'undefined') {
      return void(body.className += ' no-svg');
    }

    xhr = new XDomainRequest;
    xhr.open('GET', url);
  }

  xhr.onload = function() {
    div.className = 'u-visually-hidden';
    div.innerHTML = xhr.responseText;
  };

  xhr.onerror = function() {
    body.className += ' no-svg';
  };

  setTimeout(function() {
    xhr.send();
  }, 0);
})('/img/sprite.svg');
```

### <use> in your markup

```html
<svg role="img">
  <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#svg-{{ name }}"/>
</svg>

```

## Contributing

First of all, **thank you** for contributing, **you are awesome**.

Here are a few rules to follow in order to ease code reviews, and discussions before maintainers accept and merge your work:

- Make sure your commit messages make sense (don't use `fix tests`, `small improvement`, `fix 2`, among others).
- Before creating a pull request make sure of the following:
    - your code is all documented properly;
    - your code passes the ESLint rules;
    - variable, function and class names are explanatory enough;
    - code is written in ES2015.
- When creating a pull request give it a name and description that are explanatory enough. In the description detail everything you are adding, do not assume we will understand it from the code.

Thank you!

---

Copyright (c) 2017 [Matt Stow](http://mattstow.com)  
Licensed under the MIT license *(see [LICENSE](https://github.com/stowball/react-accessible-tabs/blob/master/LICENSE) for details)*
