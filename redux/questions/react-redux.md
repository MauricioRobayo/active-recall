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