# 🎮  Consola

[![Standard JS][standard-js-src]][standard-js-href]
[![npm version][npm-version-src]][npm-version-href]
[![npm downloads][npm-downloads-src]][npm-downloads-href]

> Elegant Console Logger

## Why Consola?

- Easy to use
- Fancy output with fallback for continuous integration (CI) environments
- Global mockable stdout/stderr wrapper
- Pluggable reporters
- Consistent command line interface (CLI) experience
- Tag support

!!! Note: Consola v2 is stil under development. Meanwhile, you may want to use [1.x](https://github.com/nuxt/consola/tree/1.x) branch docs.

## Installation

Using yarn:

```bash
yarn add consola
```

Using npm:

```bash
npm i consola
```

## Getting Started

```js
const consola = require('consola')

// See types section for all available types

consola.success('Built!')
consola.info('Reporter: Some info')
consola.error(new Error('Foo'))
```

<div align="center">
<br>
<img src="./assets/fancy.png">
<p>Fancy Reporter</p>
<br>
</div>

<pre>
[15:21:29] [nuxt:router] [DEBUG  ] consola log
[15:21:29] [nuxt:router] [ERROR  ] consola log
[15:21:29] [nuxt:router] [FATAL  ] consola log
[15:21:29] [nuxt:router] [INFO   ] consola log
</pre>
<div align="center">
  <p>Minimal Reporter (CI)</p>
  <br>
</div>

## Methods

#### `<type>(logObject)`
#### `<type>(args...)`

Log to all reporters.

#### `addReporter(reporter)`

Register a custom reporter instance.

#### `removeReporter(reporter?)`

Remove a registered reporter.

If no arguments are passed all reporters will be removed.

#### `setReporters(reporter|reporter[])`

- Type: `Object` or `Array`

Replace all reporters.

#### `create(options)`

Create a new `Consola` instance and inherit all parent options for defaults.

#### `withDefaults(defaults)`

Create a new `Consola` instance with provided defaults

#### `withTag(tag)`

Create a new `Consola` instance with that tag.

#### `wrapConsole()`
#### `restoreConsole()`

Globally redirect all `console.log`, etc calls to consola handlers.

#### `pause()`
#### `resume()`

**Globally** pause and resume logs.

Consola will enqueue all logs when paused and then sends them to the reported when resumed.

## Fields

#### `reporters`

An array of active reporters.

#### `level`

The level to display logs. Any logs at or above this level will be displayed.
List of available levels [here](./src/types.js).

You can set log level using `CONSOLA_LEVEL` environment variable.

## logObject

logObject is a free-to-extend object which will be passed to reporters.

Standard fields:

- `additional`
- `additionalColor`
- `args`
- `date`
- `icon`
- `message`
- `tag`

## Reporters

Choose between one of the built-in reporters or bring own reporter.

By default `FancyReporter` is registered for modern terminals or `BasicReporter` will be used if running in limited environments such as CIs.

Available reporters:

- [BasicReporter](./src/reporters/basic.js)
- [FancyReporter](./src/reporters/fancy.js)
- [JSONReporter](./src/reporters/json.js)
- [WinstonReporter](./src/reporters/winston.js)

### Creating your own reporter

A reporter (Class or Object) exposes `log(logObj)` method.
To write a reporter, check implementations to get an idea.

## Types

Types are _logging levels_. A list of all available default types is [here](./src/types.js).

## Creating a new instance

Consola has a global instance and is recommended to use everywhere.
In case more control is needed, create a new instance.

```js
import consola from 'consola'

const logger = consola.create({
    // level: 4,
    reporters: [
      new consola.JSONReporter()
    ],
    defaults: {
      additionalColor: 'white'
    }
})
```

## Integrations

### With jest

```js
consola.setReporters({
  log: jest.fn()
})
```

### With jsdom

```js
{
  virtualConsole: new jsdom.VirtualConsole().sendTo(consola)
}
```

## License

MIT - Made with 💖 By Nuxt.js team!

<!-- Refs -->
[standard-js-src]: https://flat.badgen.net/badge/code%20style/standard/green
[standard-js-href]: https://standardjs.com
[npm-version-src]: https://flat.badgen.net/npm/v/consola/latest
[npm-version-href]: https://npmjs.com/package/consola
[npm-downloads-src]: https://flat.badgen.net/npm/dt/consola
[npm-downloads-href]: https://npmjs.com/package/consola
