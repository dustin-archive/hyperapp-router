# Google Analytics

Adding Google Analytics to this router is easy peasy.

1. Add the Google Analytics library and snippet in the head of your document.
1. Create the connection using your Google Analytics tracking ID.
1. Make a callback function for router's `init` using the Google Analytics library to send the current path.

More information can be found here: https://developers.google.com/analytics/devguides/collection/analyticsjs/

```html
<script async src='https://www.google-analytics.com/analytics.js'></script>
```

```js
window.ga = window.ga || (item => {
  (window.ga.q = window.ga.q || []).push(item)
})

window.ga.l = new Date().getTime()

window.ga(['create', 'UA-XXXXXXXXX-X', 'auto'])

const googleAnalytics = state => {
  ga(['send', 'pageview', state.path])
}

hyperapp.Router.init(googleAnalytics)

window.addEventListener('hashchange', e => {
  hyperapp.Router.init(googleAnalytics)
})
```
