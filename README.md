# Koa Trie Router

[![NPM version][npm-image]][npm-url]
[![build status][travis-image]][travis-url]
[![Test coverage][coveralls-image]][coveralls-url]
[![Gittip][gittip-image]][gittip-url]

[Trie](http://en.wikipedia.org/wiki/Trie) routing for Koa based on [routington](https://github.com/jonathanong/routington).

## Features

- `OPTIONS` support
- `405 Method Not Allowed` support
- `501 Not Implemented` support

Routes are generally orthogonal, so the order of definition generally doesn't matter.
See [routington](https://github.com/jonathanong/routington) for more details.

## Installation

```js
var app = require('koa')()
app.use(require('koa-trie-router')(app))

app.route('/')
.get(function* (next) {
  this.body = 'homepage'
})

app.post('/images', function* (next) {
  var image = yield* this.request.buffer('25mb')
})
```

## API

### this.assertImplementsMethod()

Checks if the server implements a particular method and throws a `501` error otherwise.
This is not middleware, so you would have to use it in your own middleware.

```js
app.use(myCustomErrorHandler)

app.use(function* (next) {
  this.request.assertImplementsMethod() // throws otherwise
  yield next
})
```

### app.use(app.router)

Like Express, all routes belong to a single middleware.
Unlike Express, `app.router` is not implicitly mounted.
If you do not do `app.use(app.router)` ever,
routing will never work.

### app.route(paths)\[method\]\(middleware...\)

`paths` can be a nested stack of string paths:

```js
app.route('/one', [
  '/two',
  ['/three', '/four']
])
```

You can then chain `[method](middleware...)` calls.

```js
app.route('/')
.get(function* (next) {

})
.post(function* (next) {

})
.patch(function* (next) {

})
```

### app\[method\]\(paths, middleware...\)

Similar to above, but you define `paths` as the first argument:

```js
app.get([
  '/one',
  '/two'
], function* (next) {

})
```

### this.params

`this.params` will be defined with any matched parameters.

```js
app.get('/user/:name', function* (next) {
  var name = this.params.name
  var user = yield User.get(name)
  yield next
})
```

### Path Definitions

For path definitions, see [routington](https://github.com/jonathanong/routington).

## Usage

In `trie-router`, routes are orthogonal and strict. Unlike regexp routing, there's no wildcard routing and you can't `next` to the next matching route.

[npm-image]: https://img.shields.io/npm/v/koa-trie-router.svg?style=flat
[npm-url]: https://npmjs.org/package/koa-trie-router
[travis-image]: https://img.shields.io/travis/koajs/trie-router.svg?style=flat
[travis-url]: https://travis-ci.org/koajs/trie-router
[coveralls-image]: https://img.shields.io/coveralls/koajs/trie-router.svg?style=flat
[coveralls-url]: https://coveralls.io/r/koajs/trie-router?branch=master
[gittip-image]: https://img.shields.io/gittip/jonathanong.svg?style=flat
[gittip-url]: https://www.gittip.com/jonathanong/
