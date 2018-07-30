# External SVG Sprite

[![npm version](https://badge.fury.io/js/external-svg-symbols-loader.svg)](https://badge.fury.io/js/external-svg-symbols-loader)
[![Build Status](https://travis-ci.org/artem-malko/external-svg-symbols-loader.svg?branch=master)](https://travis-ci.org/artem-malko/external-svg-symbols-loader)

A loader and plugin for webpack that converts all your SVGs into symbols and merges them into a SVG sprite.

## Requirements

You will need NodeJS v6+, npm v3+ and webpack >= 4.

To make it work in older browsers, like Internet Explorer, you will also need [SVG for Everybody](https://github.com/jonathantneal/svg4everybody) or [svgxuse](https://github.com/Keyamoon/svgxuse).

## Installation

```bash
npm i external-svg-symbols-loader
```

## Options

### Loader options

- `name` - relative path to the sprite file (default: `img/sprite.svg`). The `[hash]` placeholder is supported.
- `iconName` - name for the icon symbol (default: `icon-[name]-[hash:5]`).
- `svgoOptions` - custom options to be passed to svgo.

### Plugin options

- `emit` - determines if the sprite is supposed to be emitted (default: true). Useful when generating server rendering bundles where you just need the SVG sprite URLs but not the sprite itself.

## Usage

If you have the following webpack configuration:

```js
// webpack.config.js

import path from 'path';
import SvgStorePlugin from 'external-svg-symbols-loader/lib/SvgStorePlugin';

module.exports = {
    module: {
        rules: [
            {
                loader: 'external-svg-symbols-loader',
                test: /\.svg$/,
            },
        ],
    },
    ...
    plugins: [
        new SvgStorePlugin(),
    ],
};
```

You will be able to import your SVG files in your JavaScript files as shown below.
The imported SVG will always correspond to a JavaScript object with keys `symbol`, `view` and `viewBox`:
- The `symbol` url can be used on a `<use>` tag to display the icon;
- The `view` url is supposed to be used in CSS;
- The `viewBox` value is required by some browsers on the `<svg>` tag.

The URLs will have the following format:
- `symbol`: `webpackConfig.output.publicPath`/`loader.name`#`loader.iconName`
- `view`: `webpackConfig.output.publicPath`/`loader.name`#view-`loader.iconName`

```js
/*
 * {
 *  symbol: '/public/img/sprite.svg#icon-logo',
 *  view: '/public/img/sprite.svg#view-icon-logo',
 *  viewBox: '0 0 150 100'
 * }
 */
import logo from './images/logo.svg';

class {

    render() {
        return (
            <svg viewBox={logo.viewBox}>
                <use xlinkHref={logo.symbol} />
            </svg>
        );
    }

}
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

## License

MIT (http://www.opensource.org/licenses/mit-license.php)
