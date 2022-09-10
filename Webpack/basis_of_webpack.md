# Code Spilitting

- **Entry Points:** Manually split code using entry configuration.
- **Prevent Duplication:** Use Entry dependencies or SplitChunksPlugin to dedupe and split chunks.
- **Dynamic Imports:** Split code via inline function calls within modules.

## Entry Points

```js
const path = require('path');

module.exports = {
  mode: 'development',
  entry: {
    index: {
      import: './src/index.js',
      dependOn: 'shared',
    },
    another: {
      import: './src/another-module.js',
      dependOn: 'shared',
    },
    shared: 'lodash',
  },
  output: {
    filename: '[name].bundle.js',
    path: path.resolve(__dirname, 'dist'),
  },
};
```

If we're going to use multiple entry points on a single HTML page, optimization.runtimeChunk: 'single' is needed too, otherwise we could get into trouble described [here](https://bundlers.tooling.report/code-splitting/multi-entry/).

```js
const path = require('path');

module.exports = {
  mode: 'development',
  entry: {
    index: {
      import: './src/index.js',
      dependOn: 'shared',
    },
    another: {
      import: './src/another-module.js',
      dependOn: 'shared',
    },
    shared: 'lodash',
  },
  output: {
    filename: '[name].bundle.js',
    path: path.resolve(__dirname, 'dist'),
  },
  optimization: {
    runtimeChunk: 'single',
  },
};
```

:::tip
Although using multiple entry points per page is allowed in webpack, it should be avoided when possible in favor of an entry point with multiple imports: entry: { page: ['./analytics', './app'] }. This results in a better optimization and consistent execution order when using async script tags.
:::

https://webpack.js.org/concepts/entry-points/#multi-page-application

## SplitChunksPlugin

https://webpack.js.org/plugins/split-chunks-plugin/

With the optimization.splitChunks configuration option in place, we should now see the duplicate dependency removed from our index.bundle.js and another.bundle.js. The plugin should notice that we've separated lodash out to a separate chunk and remove the dead weight from our main bundle.

```js
const path = require('path');

module.exports = {
  mode: 'development',
  entry: {
    index: './src/index.js',
    another: './src/another-module.js',
  },
  output: {
    filename: '[name].bundle.js',
    path: path.resolve(__dirname, 'dist'),
  },
  optimization: {
    splitChunks: {
      chunks: 'all',
    },
  },
};
```

other useful plugins

- **mini-css-extract-plugin:** Useful for splitting CSS out from the main application.

## Dynamic Imports

`import()`

https://webpack.js.org/api/module-methods/#import-1

## Prefetching/Preloading modules

Webpack 4.6.0+ adds support for prefetching and preloading.

Using these inline directives while declaring your imports allows webpack to output “Resource Hint” which tells the browser that for:

- prefetch: resource is probably needed for some navigation in the future
- preload: resource will also be needed during the current navigation

Preload directive has a bunch of differences compared to prefetch:

- A preloaded chunk starts loading in parallel to the parent chunk. A prefetched chunk starts after the parent chunk finishes loading.
- A preloaded chunk has medium priority and is instantly downloaded. A prefetched chunk is downloaded while the browser is idle.
- A preloaded chunk should be instantly requested by the parent chunk. A prefetched chunk can be used anytime in the future.
- Browser support is different.
