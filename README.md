# @whaaaley/hyperapp-router

> Minimal hash router

Routers are great!
This one only has paths and query strings.
To use it you have to create a router view function yourself.
You can't get more bare bones than this.

## Install

```sh
npm i @whaaaley/hyperapp-router
```

## Setup

Setting up the router is easy.
This example contains everything you need.

```js
import { h, app } from 'hyperapp'
import { Router } from 'hyperapp-starterkit'

const state = {
  Router: {}
}

const actions = {
  Router
}

const view = {
  // ...
}

const hyperapp = app(state, actions, view)

hyperapp.Router.init()

window.addEventListener('hashchange', e => {
  hyperapp.Router.init()
})
```

## Usage

### Mapping Paths to Views

There's no built-in way to change your view based on the current path.
The recommended way to change your view is to create a "router view" function similar to the following example.
It's incredibly simple and it gets the job done!
As your app grows in complexity you may want to opt for a more featured router library, but simple is often enough!

```js
import { h, app } from 'hyperapp'

const Home = h('div', null, 'Home')
const About = h('div', null, 'About')
const Contact = h('div', null, 'Contact')
const NotFound = h('div', null, '404')

const RouterView = path => ({
  '': Home,
  '/about': About,
  '/contact': Contact
})[path] || NotFound

const state = {
  // ...
}

const actions = {
  // ...
}

const view = d => state => RouterView(state.Router.path)

app(state, actions, view)
```

### Routing

Paths and queries can be updated manually, like in the address bar or with an anchor tag, or you can programmatically set path and queries using the `route` action.
State should always reflect both of these methods.

```js
actions.Router.route({
  path: '/about',
  query: {
    fuzzy: 'wuzzy',
    was: 'a',
    bear: 'fuzzy',
    wuzzy: 'had',
    no: 'hair'
  }
})
// => http://localhost:3000/#/about?fuzzy=wuzzy&was=a&bear=fuzzy&wuzzy=had&no=hair

state.Router.path
// => /about

state.Router.query
// => { fuzzy: 'wuzzy', was: 'a', bear: 'fuzzy', wuzzy: 'had', no: 'hair' }
```

### Integrating Things

The `init` action optionally accepts a callback with an argument of the router's state.
This is useful when adding libraries that depend on your routes, like analytics.

```js
const optionalCallback = routerState => {
  // ...
}

hyperapp.Router.init(optionalCallback)

window.addEventListener('hashchange', e => {
  hyperapp.Router.init(optionalCallback)
})
```
