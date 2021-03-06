rollup-plugin-iife
==================

[![Build Status](https://travis-ci.org/eight04/rollup-plugin-iife.svg?branch=master)](https://travis-ci.org/eight04/rollup-plugin-iife)
[![codecov](https://codecov.io/gh/eight04/rollup-plugin-iife/branch/master/graph/badge.svg)](https://codecov.io/gh/eight04/rollup-plugin-iife)
[![install size](https://packagephobia.now.sh/badge?p=rollup-plugin-iife)](https://packagephobia.now.sh/result?p=rollup-plugin-iife)

Currently (rollup@0.65), [rollup doesn't support code splitting with IIFE output](https://github.com/rollup/rollup/issues/2072). This plugin would transform ES module output into IIFEs.

Installation
------------

```
npm install -D rollup-plugin-iife
```

Usage
-----

```js
import iife from "rollup-plugin-iife";

export default {
  input: ["entry.js", "entry2.js"],
  output: {
    dir: "dist",
    format: "es",
    globals: {
      vue: "Vue"
    }
  },
  externals: ["vue"],
  plugins: [iife()]
};
```

Define global variables for external imports
--------------------------------------------

You can define global variables with `output.globals` just like before. You can also specify them with the plugin option `names`.

The plugin would first lookup `names` option then `output.globals`.

API
----

This module exports a single function.

### createPlugin

```js
const plugin = createPlugin({
  names?: Function|Object,
  sourcemap?: Boolean
});
```

Create the plugin instance.

If `names` is a function, the signature is:

```js
(moduleId: String) => globalVariableName: String
```

If `names` is an object, it is a `moduleId`/`globalVariableName` map. `moduleId` can be relative to the output folder (e.g. `./entry.js`), the plugin would resolve it to the absolute path.

If the plugin can't find a proper variable name, it would generate one according to its filename with [camelcase](https://www.npmjs.com/package/camelcase).

If `sourcemap` is false then don't generate the sourcemap. Default: `true`.

Related projects
----------------

* [rollup-plugin-external-globals](https://www.npmjs.com/package/rollup-plugin-external-globals) - transform imports into global variables *at transform hook*.
* [amd-script](https://www.npmjs.com/package/amd-script) - a small AMD module cache that would work with AMD module chunks generated by Rollup. You have to load modules manually (e.g. load modules through `<script>` tags).
* [rollup-plugin-loadz0r](https://github.com/surma/rollup-plugin-loadz0r) - attach each chunk with a module loader so you don't have to include a module loader at runtime. Support workers.

Changelog
---------

* 0.1.1 (Sep 28, 2018)

  - Add: support `output.globals` option.

* 0.1.0 (Aug 28, 2018)

  - First release.
