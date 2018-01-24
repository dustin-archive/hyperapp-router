# Reference

## `action.init()`

The `init` action updates the router's state from `window.location.hash`.

### Syntax

```js
// actions.init([callback])

actions.init(state => {
  console.log(state)
})
```

### Parameters

`callback` (optional) - A `Function` called before the router's state is updated. This function has a `state` argument which contains the router's new state.

## `action.route()`

The `route` action updates the path or query in `window.location.hash` from the object passed in. If no path or no query is passed, then the path or query is pulled from state.

### Syntax

```js
// actions.route([route])

actions.route({
  path: '/foobar',
  query: {
    foo: 'bar'
  }
})
```

### Parameters

`route` (optional) - An options object detailing the new route.
  + `path` (optional): A path string
  + `query` (optional): A query object
