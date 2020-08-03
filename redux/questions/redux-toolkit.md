# Redux Toolkit

## `configureStore`

<details>
  <summary>What does `configureStore` do?</summary>
  <br/>

  Creates a Redux store instance like the original `createStore` from Redux, but accepts a named options object and sets up the Redux DevTools Extension automatically.

</details>
<details>
  <summary>What are the main benefits of using `configureStore` over `createStore`?</summary>
  <br/>

  - Having an options object with "named" paramenters, which can be easier to read.
  - Letting you provide arrays of middleware and enhancers you want to add to the store, and calling `applyMiddleware` and `compose` for you automatically.
  - Enabling the Redux DevTools Extension automatically.

</details>
<details>
  <summary>Which middleware is added by default with `configureStore`?</summary>
  <br/>

  - `redux-thunk` is the most commonly used middleware for working with both synchronous and async logic outside of components.
  - In development, middleware that check for common mistakes like mutating the state or using non-serializable values.

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
  <summary>What additional argument does `createAction` accepts?</summary>
  <br/>

  It also accepts a "prepare callback" argument, which allow you to customize the resulting `payload` field and optionally add a `meta` and/or `error` field.

</details>
<details>
  <summary>What kind of information can the `meta` field contain?</summary>
  <br/>

  The optional `meta` property MAY be any type of value. It is intended for any extra information that is not part of the payload.

</details>
<details>
  <summary>What kind of information can the `error` field contain?</summary>
  <br/>

  
  The optional `error` property MAY be set to `true` if the actions represents an error.

  An actions whose `error` property is true is analogous to a rejected Promise. By convention, the `payload` SHOULD be an error object.

  If `error` has any other value besied `true`, including `undefined` and `null`, the action MUST NOT be interpreted as an error.

</details>
<details>
  <summary>What specification does `createAction` follows for it's fields?</summary>
  <br/>

  The [Flux Standard Actions](https://github.com/redux-utilities/flux-standard-action#actions).

</details>
<details>
  <summary>What are the two conditions an `action` MUST have according to the "Flux Standard Actions"?</summary>
  <br/>

    - be a plain JavaScript object.
    - have a `type` property.

</details>
<details>
  <summary>What are the three properties an `action` MAY have according to the "Flux Standard Actions"?</summary>
  <br/>

  - an `error` property.
  - a `payload` property.
  - a `meta` property.

</details>

## `createReducer`

<details>
  <summary>What are the two main benefits of `createReducer`?</summary>
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
<details>
  <summary>What's the main consideration while using `immer` in `createReducer`?</summary>
  <br/>

  You need to ensure that you either mutate the `state` argument or return a new state, but not both.

  For example, this is fine as this returns a new argument:

  ```js
  const visibilityFilter = createReducer(VisibilityFilters.SHOW_ALL, {
    [setVisibilityFilter]: (state, action) => action.payload 
  })
  ```

  But this is not fine, as this mutates the state **and** returns a new state (be careful with the use of arrow functions):

  ```js
  const todos = createReducer([], {
    [addTodo]: (state, action) => state.push({
        id: action.payload.id,
        text: action.payload.text,
        completed: false
      }),
    [toggleTodo]: (state, action) => state[action.payload].completed = !state[action.payload].completed
  })
  ```

  To fix this, either:
  
  - wrap the assignment in curly braces to make it a function body
  - add the void operator in front of the assignment

  ```js
  const todos = createReducer([], {
    // wrap the assignment in curly braces
    [addTodo]: (state, action) => {
      state.push({
        id: action.payload.id,
        text: action.payload.text,
        completed: false
      })
    },
    // use the void operator
    [toggleTodo]: (state, action) => void(state[action.payload].completed = !state[action.payload].completed),
  })
  ```

</details>

## `createSlice`

<details>
  <summary>What's a `slice`?</summary>
  <br/>

  A normal Redux application has a JS object at the top of its state tree, and that object is the result of calling the Redux [`combineReducers` function](https://redux.js.org/api/combinereducers) to join multiple reducer functions into one large "root reducer".
  
  **We refer to one key/value section of that object as a "slice", and we use the term "[slice reducer](https://redux.js.org/recipes/structuring-reducers/splitting-reducer-logic)" to describe the reducer function responsible for updating that slice of the state**.

</details>
<details>
  <summary>What does `createSlice` returns?</summary>
  <br/>

  Returns a "slice" object that contains the generated reducer function as a field named `reducer`, and the generated action creators inside an object called `actions`.

</details>

## `createAsyncThunk`

https://redux-toolkit.js.org/usage/usage-guide#asynchronous-logic-and-data-fetching

## `createEntityAdapter`

https://redux-toolkit.js.org/usage/usage-guide#managing-normalized-data