# @whaaaley/hyperapp-router

> Minimal hash router

Routers are great!
This one only has paths and query strings.
To use it you have to create a router view function yourself.
You can't get more barebones than this.

## Install

```sh
npm i @whaaaley/hyperapp-router
```

## Router Installation

Installing the router is easy.
This example contains everything you need to install the router.

```js
import { app } from 'hyperapp'
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

## Router Usage

### Mapping Paths to Views

There's no built-in way to change your view based on the current path.
The reccomended way to change your view is to create a "router view" function similar to the following example.
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

### Changing Routes

Path and queries can be updated in the address bar or with an anchor tag.
You can also programmatically set path and queries using the `route` action.
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

### Integrating Libraries

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

## Additional Information

### Google Analytics

Adding Google Analytics to this router is easy peasy.

1. Add the Google Analytics library and snippet in the head of your document.
1. Create the connection using your Google Analytics tracking ID.
1. Make a callback function for router's `init` using the Google Analytics library to send the current path.

More information can be found here: https://developers.google.com/analytics/devguides/collection/analyticsjs/

```html
<script async src='https://www.google-analytics.com/analytics.js'></script>
```

```js
const ga = window.ga || (item => {
  (window.ga.q = window.ga.q || []).push(item)
})

ga.l = new Date().getTime()

ga(['create', 'UA-XXXXXXXXX-X', 'auto'])

const googleAnalytics = state => {
  ga(['send', 'pageview', state.path])
}

hyperapp.Router.init(googleAnalytics)

window.addEventListener('hashchange', e => {
  hyperapp.Router.init(googleAnalytics)
})
```
