ArkhamJS Skeleton App
=====================

A simple skeleton to start you off on your ReactJS project. Uses [ArkhamJS](https://github.com/nitrogenlabs/arkhamjs) framework for a flux architecture.

[![Travis](https://img.shields.io/travis/nitrogenlabs/arkhamjs-skeleton.svg?style=flat-square)](https://travis-ci.org/nitrogenlabs/arkhamjs-skeleton)
[![Issues](http://img.shields.io/github/issues/nitrogenlabs/arkhamjs-skeleton.svg?style=flat-square)](https://github.com/nitrogenlabs/arkhamjs-skeleton/issues)
[![Gitter](https://img.shields.io/gitter/room/NitrgenLabs/arkhamjs.svg?style=flat-square)](https://gitter.im/NitrogenLabs/arkhamjs)
[![MIT license](http://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square)](http://opensource.org/licenses/MIT)

Getting Started
---------------

- Clone the repo and install the necessary node modules:

```shell
$ npm install                   # Install Node modules listed in ./package.json (may take a while the first time)
$ npm install -g nodemon        # Install Nodemon
$ gulp                          # Compile and launch
```

Usage
-----

#### `gulp` also `gulp dev`
Runs the webpack build system just like in `compile` but enables HMR. The webpack dev server can be found at `localhost:3000`.

#### `gulp compile`
Runs the Webpack build system with your current NODE_ENV and compiles the application to disk (`~/dist`). Production builds will fail on eslint errors (but not on warnings).

#### `gulp test`
Runs unit tests with Karma.

#### `gulp deploy`
Helper script to run tests and then, on success, compile your application.

### Configuration

Basic project configuration can be found in `~/build/config.js`. Here you'll be able to redefine your src and dist directories, as well as tweak what ports Webpack and WebpackDevServer run on.

Structure
---------

The folder structure provided is only meant to serve as a guide, it is by no means prescriptive. It is something that has worked very well for me and my team, but use only what makes sense to you.

```
.
├── build                    # All build-related configuration
│   ├── tasts                # Gulp configurations
│   ├── config.js            # Project configuration settings
│   └── dev-server.js        # Development server configuration
│   └── prod-server.js       # Production server configuration
├── coverage                 # Unit test coverage reports
├── dist                     # Compiled files
│   ├── dev                  # Development files
│   └── prod                 # Production files
├── js                       # External js files
├── src                      # Application source code
│   ├── actions              # Flux actions
│   ├── components           # React components
│   ├── config               # Configuration files
│       ├── development      # Development configuration
│       └── production       # Production configuration
│   ├── constants            # Collection of constants used in routes and events
│   ├── errors               # Custom errors
│   ├── fonts                # Font files
│   ├── icons                # SVG files
│   ├── img                  # Images
│   ├── services             # Helpers and utilities
│   ├── stores               # Redux store configuration
│   ├── styles               # SCSS styles
│   ├── views                # React components/views that live at a route
│   └── index.js             # Application bootstrap and rendering
└── test                     # Test specs
```

### Components vs. Views vs. Layouts

**TL;DR:** They're all components.

This distinction may not be important for you, but as an explanation: A **Layout** is something that describes an entire page structure, such as a fixed navigation, viewport, sidebar, and footer. Most applications will probably only have one layout, but keeping these components separate makes their intent clear. **Views** are components that live at routes, and are generally rendered within a **Layout**. What this ends up meaning is that, with this structure, nearly everything inside of **Components** ends up being a dumb component.

Webpack
-------

### Configuration
The webpack compiler configuration is located in `~/build/webpack`. When the webpack dev server runs, only the client compiler will be used. When webpack itself is run to compile to disk, both the client and server configurations will be used. Settings that are bundle agnostic should be defined in `~/build/config.js` and imported where needed.

### Vendor Bundle
You can redefine which packages to treat as vendor dependencies by editing the vendor property in the webpack config in `~/build/config.js`. These default to:

```js
[
  'arkhamjs',
  'babel-polyfill',
  'bluebird',
  'react',
  'react-dom',
  'react-router',
  'whatwg-fetch'
]
```

### Aliases
As mentioned in features, the default Webpack configuration provides some globals and aliases to make your life easier. These can be used as such:

```js
import MyComponent from '../../components/my-component'; // without alias
import MyComponent from 'components/my-component'; // with alias

  // Available aliases:
  actions     => '~/src/actions'
  components  => '~/src/components'
  config      => '~/src/config/[env]'
  constants   => '~/src/constants'
  errors      => '~/src/errors'
  services    => '~/src/services'
  stores      => '~/src/stores'
  styles      => '~/src/styles'
  test        => '~/test'
  views       => '~/src/views'
```

### Globals

#### `__DEV__`
True when `process.env.NODE_ENV` is `development`

#### `__PROD__`
True when `process.env.NODE_ENV` is `production`

#### `__DEBUG__`
True when the compiler is run with `--debug` (any environment).

Styles
------

All `.scss` imports will be run through the sass-loader, extracted during production builds, and ignored during server builds. If you're requiring styles from a base styles directory (useful for generic, app-wide styles) in your JS, you can make use of the `styles` alias, e.g.:

```js
// ~/src/components/some/nested/component/index.jsx
import `styles/core.scss`;
```

Furthermore, this `styles` directory is aliased for sass imports, which further eliminates manual directory traversing. An example nested `.scss` file:

```scss
// current path: ~/src/styles/some/nested/style.scss
// what used to be this:
@import '../../base';

// can now be this:
@import 'base';
```

Testing
-------

To add a unit test, simply create `.spec.js` file in `~/test/unit`. All imports will be relative to the "~/src" directory. The entry point for Karma uses webpack's custom require to load all these files, and Jasmine will be available to you within your test without the need to import them.

Troubleshooting
---------------

Nothing yet. Having an issue? Report it and We'll get to it as soon as possible!
