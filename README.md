# postcss-custom-properties [![Build Status](https://travis-ci.org/postcss/postcss-custom-properties.png)](https://travis-ci.org/postcss/postcss-custom-properties)

> [PostCSS](https://github.com/postcss/postcss) plugin to transform [W3C CSS Custom Properties for cascading variables](http://www.w3.org/TR/css-variables/) syntax to more compatible CSS.

**N.B.** The transformation _is not complete and **cannot be** (dynamic variables based on custom properties relies on the DOM tree)_. It currently just aims to provide a future-proof way of using a **limited subset (to top-level `:root` selector)** of the features provided by native CSS custom properties.
_Read [#1](https://github.com/postcss/postcss-custom-properties/issues/1) & [#9](https://github.com/postcss/postcss-custom-properties/issues/9) to know why this limitation exists._

_If you are looking for a full support of CSS custom properties, please follow [the opened issue for runtime support](https://github.com/postcss/postcss-custom-properties/issues/32)._

**N.B.²** If you are wondering why there is a different plugin ([`postcss-css-variables`](https://github.com/MadLittleMods/postcss-css-variables)) that claims to do more than this plugin, be sure to understand the explanation above about limitation. This plugins have a behavior that is not reflecting the specifications. [You should check some (closed) issues of this plugin](https://github.com/MadLittleMods/postcss-css-variables/issues?utf8=%E2%9C%93&q=is%3Aissue+is%3Aclosed+-label%3Ainvalid+author%3AMoOx+).

_This plugin works great with [postcss-calc](https://github.com/postcss/postcss-calc)._

## Installation

```console
$ npm install postcss-custom-properties
```

## Usage

```js
// dependencies
var fs = require("fs")
var postcss = require("postcss")
var customProperties = require("postcss-custom-properties")

// css to be processed
var css = fs.readFileSync("input.css", "utf8")

// process css using postcss-custom-properties
var output = postcss()
  .use(customProperties())
  .process(css)
  .css
```

Using this `input.css`:

```css
:root {
  --color: red;
}

div {
  color: var(--color);
}
```

you will get:

```css
div {
  color: red;
}
```

Checkout [tests](test) for more.

### Options

#### `strict` (default: `true`)

Per specifications, all fallbacks should be added since we can't verify if a
computed value is valid or not.
This option allows you to avoid adding too many fallback values in your CSS.

#### `preserve` (default: `false`)

Allows you to preserve custom properties & var() usage in output.

```js
var out = postcss()
  .use(customProperties({preserve: true}))
  .process(css)
  .css
```

You can also set `preserve: "computed"` to get computed resolved custom properties in the final output.
Handy to make them available to your JavaScript.

#### `variables` (default: `{}`)

Allows you to pass an object of variables for `:root`. These definitions will override any that exist in the CSS.
The keys are automatically prefixed with the CSS `--` to make it easier to share
variables in your codebase.

#### `appendVariables` (default: `false`)

If `preserve` is set to `true` (or `"computed"`), allows you to append your variables at the end of your CSS.

---

## Contributing

Work on a branch, install dev-dependencies, respect coding style & run tests before submitting a bug fix or a feature.

```console
$ git clone https://github.com/postcss/postcss-custom-properties.git
$ git checkout -b patch-1
$ npm install
$ npm test
```

## [Changelog](CHANGELOG.md)

## [License](LICENSE)
