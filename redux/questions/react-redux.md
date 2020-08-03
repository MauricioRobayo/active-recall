# React Redux

https://react-redux.js.org/introduction/quick-start

## `mapDispatchToProps`

<details>
  <summary>What is `mapDispatchToProps` used for?</summary>
  <br/>

  As the second argument passed to `connect`, `mapDispatchToProps` is used for dispatching actions to the store.

</details>
<details>
  <summary>Which are the two ways React Redux gives you to let components dispatch actions?</summary>
  <br/>

  1. By default, a connected component receives `prop.dispatch` and can dispatch actions itself.
  2. `connect` can accept an argument called `mapDispatchToProps`, which lets you create functions that dispatch when called, and pass those functions as props to your component.

</details>
<details>
  <summary>How is `mapDispatchToProps` normally referred to?</summary>
  <br/>

  `mapDispatch`

</details>
<details>
  <summary>What are the advantages of using `mapDispatchToProps` instead of using the default `props.dispatch`?</summary>
  <br/>

  1. More declarative code:

  ```js
  // button needs to be aware of "dispatch"
  <button onClick={() => dispatch({ type: "SOMETHING" })} />

  // button unaware of "dispatch",
  <button onClick={doSomething} />
  ```

  2. Pass Down Action Dispatching Logic to (Unconnected) Child Components. This allows more components to dispatch actions, while keeping them "unaware" of Redux.

</details>
<details>
  <summary>Which are the two forms of `mapDispatchToProps`?</summary>
  <br/>

  1. **Function form**: Allows more customization, gain access to `dispatch` and optionalyy `ownProps`.
  2. **Object shorthand form**: More declarative and easier to use.

  It is recommended to use the object form of `mapDispatchToProps` unless you specifically need to customize dispatching behavior in some way.

</details>
<details>
  <summary>What are the arguments that `mapDispatchToProps` as a functions receives?</summary>
  <br/>

  1. Dispatch: The `mapDispatchToProps` function will be called with `dispatch` as the first argument. You will normally make use of this by returning new functions that call dispatch() inside themselves, and either pas in a plain action object directly or pass in the result of an action creator.
  2. ownProps (optional): if your `mapDispatchToProps` function is declared as taking two parameters, it will be called with `dispatch` as the first parameter and the `props` passed to the connected component as the second parameter, and will be re-invoked whenever the connected component receives new props.

</details>
<details>
  <summary>What does `mapDispatchToProps` should return?</summary>
  <br/>

  It should return a plain object.

  - Each field in the object will become a separate prop for your own component, and the value should normally be a function that dispatches and action when called.
  - If you use actions creators (as oppose to plain object actions) inside `dispatch`, it si a convention to simply name hte field key the same name as the actions creator.

</details>
<details>
  <summary>What happens to the returned object of `mapDispatchToProps`?</summary>
  <br/>

  It will be merged to your connected component props.

</details>
<details>
  <summary>What does `bindActionsCreators` do?</summary>
  <br/>

  Turns an object whose values are _action creators_, into an object with the same keys, but with every action creator wrapped into a `dispatch` call so they may be invoke directly.

</details>
<details>
  <summary>What are the two parameters for `bindActonCreators`?</summary>
  <br/>

  1. A `function` (an action creator) or an `object` (each field an action creator).
  2. `dispatch`.

</details>
<details>
  <summary>How is `bindActionCreators` used with `mapDispatchToProps`?</summary>
  <br/>

  ```js
  import { bindActionCreators } from 'redux'
  
  const increment = () => ({ type: 'INCREMENT' })
  const decrement = () => ({ type: 'DECREMENT' })
  const reset = () => ({ type: 'RESET' })

  const boundActionCreators = bindActionCreators(
    { increment, decrement, reset },
    dispatch
  )
  // returns
  // {
  //   increment: (...args) => dispatch(increment(...args)),
  //   decrement: (...args) => dispatch(decrement(...args)),
  //   reset: (...args) => dispatch(reset(...args)),
  // }

  function mapDispatchToProps(dispatch) {
    return boundActionCreators
  }

  // component receives props.increment, props.decrement, props.reset
  connect(
    null,
    mapDispatchToProps
  )(Counter)
  ```

</details>
<details>
  <summary>Which is the recommended way of using `mapDispatchToProps`?</summary>
  <br/>

  The recommended way is using the "object shorthand":

  ```js
    const mapDispatchToProps = {
    increment,
    decrement,
    reset
  }

  // React Redux does this for you automatically:
  // dispatch => bindActionCreators(mapDispatchToProps, dispatch)
  ```

</details>
<details>
  <summary>What are the two caveats of using the "object shorthand" of `mapDispatchToProps`?</summary>
  <br/>

  - Each field of the `mapDispatchToProps` object is assumed to be an actions creator.
  - Your component will no longer receive `dispatch` as a prop.

</details>

## `useSelector`

<details>
  <summary>What does the `useSelector` hook enables us to do?</summary>
  <br/>

  It allows to extract data from the Redux store state, using a selector function.

  The selector function should be pure since it is potentially executed multiple times and at arbitrary points in time.

</details>
<details>
  <summary>To which argument of Redux `connect` is `useSelector` approximately equivalent?</summary>
  <br/>

  The selector is approximately equivalent to the `mapStateToProps` argument to `connect`.

</details>
<details>
  <summary>When is `useSelector` run?</summary>
  <br/>

  Whenever the function component renders unless its reference hasn't changed since a previous render of the component so that a cached result can be returned by the hook without re-running the selector.

  It will also subscribe to the Redux store, and run the selector whenever an actions is dispatched.

</details>
<details>
  <summary>What are the differences between the selectors passed to `useSelector` and a `mapState` function?</summary>
  <br/>

  1. The selector may return any values as a result, not just an object. The return value of the selector will be used as the return value of the `useSelector()` hook.
  2. When an action is dispatched, `useSelector()` will do a reference comparison of the previous selector result value and the current result value. If they are different, the component will be forced to re-render. If they are the same, the component will not re-render.
  3. The selector function does not receive an `ownProps` argument. However, props can be used through closure or by using curried selector.
  4. Extra care must be taken when using memoizing selectors.
  5. `useSelector()` uses strict `===` reference equality checks by default, not shallow equality.

</details>
<details>
  <summary>What's the second argument to `useSelector`?</summary>
  <br/>

  An equality function.

  The default comparison is a strict `===` reference comparison. This is different that `connect()`, which uses shallow equality checks on the results of `mapState` calls to determine if re-rendering is needed.

  We can use the `shallowEqual` function from React-Redux as the `equalityFn` argument to `useSelector()`.

  The optional comparison function also enables using something like Lodash's `_.isEqual()` or Immutable.js's comparison capabilities.

</details>
<details>
  <summary>What will happen with `useSelector()` if a new object is return every time?</summary>
  <br/>

  With `useSelector()`, returning a new object every time will always force a re-render by default.

</details>
<details>
  <summary>What's the main difference between returning an object with `mapState` and with `useSelect()`?</summary>
  <br/>

  With `mapState`, all individual fields were returned in a combined object. It didn't matter if the return object was a new reference or not - `connect()` just compared the individual fields.

  With `useSelector()`, returning a new object every time will always force a re-render by default.

</details>
<details>
  <summary>How can we avoid re-rendering whe returning an identical object with `useSelector()`?</summary>
  <br/>

  1. Call `useSelector()` multiple times, with each call returning a single field value.
  2. Use Reselect or a similar library to create a memoized selector that returns multiple values in one object, but only returns a new object when one of the values has changed.
  3. Use the `shallowEqual` function from React-Redux as the `equalityFn` argument to `useSelector()`.

</details>

## [Using memoizing selectors](https://react-redux.js.org/api/hooks#using-memoizing-selectors)