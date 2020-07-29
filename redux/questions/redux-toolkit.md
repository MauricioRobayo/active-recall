# Redux Toolkit

## `configureStore`

<details>
  <summary>What does `configureStore` do?</summary>
  <br/>

  Creates a Redux store instance like the original `createStore` from Redux, but accepts a named options object and sets up the Redux DevTools Extension automatically.

</details>

## `createAction`

<details>
  <summary>What's the argument required by `createAction`?</summary>
  <br/>

  `createAction` accepts an action type string as an argument:

  ```js
  const increment = createAction('INCREMENT');
  ```

</details>
<details>
  <summary>What does `createAction` returns?</summary>
  <br/>

  An __action creator function__ that uses the string provided as a type.

  Yes, this means the name is a bit incorrect - we're creating an "action creator funtion", not an "action object", but it's shorter and easier to remember that createActionCreator.

  ```js
  const increment = createAction('INCREMENT');
  console.log(increment())
  // {type: "INCREMENT"
  ```

</details>
<details>
  <summary>How can we reference the action type string passed to an `actionCreator`?</summary>
  <br/>

  1. The action creator's `toString()` method has been overridden, and will return the action type string.
  2. The type string is also available as a `.type` field on the function.

  ```js
  const increment = createAction('INCREMENT')

  console.log(increment.toString())
  // "INCREMENT"

  console.log(increment.type)
  // "INCREMENT"
  ```

</details>
<details>
  <summary>Question goes here</summary>
  <br/>

  This is the hidden answer

</details>

## `createReducer`

<details>
  <summary>What are the two main benefit of `createReducer`?</summary>
  <br/>

  1. Simplifies creating Redux reducer functions, by defining them as lookup tables of functions to handle each action type.
  2. It allows you to drastically simplify immutable update logic, by writing "mutative" code inside your reducers.

</details>
<details>
  <summary>What are the two required arguments of `createReducer`</summary>
  <br/>

  1. `initialState`: The initial state that should be used when the reducer is called the first time.
  2. `caseReducers`: An object mapping from action types to _case reducers_, each of which handles one specific action type.

  ```js
  const counterReducer = createReducer(0, {
    increment: (state, action) => state + action.payload,
    decrement: (state, action) => state - action.payload,
  })
  ```

</details>
<details>
  <summary>What's the alternative to the `caseReducers` argument and why is it important?</summary>
  <br/>

  The second argument may be a "builder callback" function that can be used to add case handlers for specific action types, match against a range of action types, or provide a fallback default case if no other actions matched:

  ```js
  const initialState = {
    counter: 0,
    rejectedActions: 0,
    unhandledActions: 0,
  }

  const exampleReducer = createReducer(initialState, builder => {
    builder
      .addCase('counter', state => [
        state.counter++
      })
      .addMatcher(
        action => action.type.endsWith('/rejected'),
        (state, action) => {
          state.rejectedActions++
        }
      )
      .addDefaultCase((state, action) => {
        state.unhandledActons++
      }
  })
  ```

  __Note__: If you are using TypeScript, it is highly recommended to use the builder callback API to get proper inference of TS types for acton objects.

</details>
<details>
  <summary>What are the two optional arguments of `createReducer`?</summary>
  <br/>

  1. `actionMatchers`: An optional array of objects that include a `matcher` function to determine if an action should be handled, and a `reducer` function that updated the state. This argument will be ignored if the second argument is a builder callback.
  2. `defaultCase`: A reducer that will be run if no other case reducers or matchers handle a given action. This argument will be ignored if the seconde argument is a builder callback.

</details>
<details>
  <summary>How does `createReducer` enables you to write reducers as if they were mutating the state directly?</summary>
  <br/>

  `createReducer` uses the [immer](https://github.com/mweststrate/immer) library to let you write reducers as if they were mutating the state directly. In reality, the reducer receives a proxy state that translates all mutations into equivalent copy operations.

</details>

## `createSlice`

<details>
  <summary>What does `createSlice` returns?</summary>
  <br/>

  Returns a "slice" object that contains the generated reducer function as a field named `reducer`, and the generated action creators inside an object called `actions`.

</details>