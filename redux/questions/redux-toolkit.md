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

## TypeScript

<details>
  <summary>What is the easiest way of getting the `State` type?</summary>
  <br/>

  Define the root reducer in advance and extract its ReturnType.

  ```ts
  import { combineReducers } from '@reduxjs/toolkit'
  const rootReducer = combineReducers({})
  export type RootState = ReturnType<typeof rootReducer>
  ```

</details>
<details>
  <summary>Why is not recommended to give the `State` type the name of `State`?</summary>
  <br/>

  Because the type name `State` is usually overused, so it is recommended to give the `State` type a different name like `RootState`.

</details>
<details>
  <summary>How can you get the type of the `State` without using a `rootReducer` and directly using `configureStore`?</summary>
  <br/>

  ```ts
  import { configureStore } from '@reduxjs/toolkit'
  // ...
  const store = configureStore({
    reducer: {
      one: oneSlice.reducer,
      two: twoSlice.reducer
    }
  })
  export type RootState = ReturnType<typeof store.getState>
  ```

</details>
<details>
  <summary>How can you extract the `Dispatch` type from the store?</summary>
  <br/>

  ```ts
  import { configureStore } from '@reduxjs/toolkit'
  import rootReducer from './rootReducer'

  const store = configureStore({
    reducer: rootReducer
  })

  export type AppDispatch = typeof store.dispatch
  export const useAppDispatch = () => useDispatch<AppDispatch>() // Export a hook that can be reused to resolve types
  ```

</details>
<details>
  <summary>Which is the suggested notation for the `middleware` option to get a correctly pre-typed version of `getDefaultMiddleware` that does not require you to specify any generics by hand?</summary>
  <br/>

  Using the callback notation for the `middleware` option to get a correctly pre-types version of `getDefaultMiddleware` that does not require you to specify any generics by hand.

  ```ts
  import { configureStore } from '@reduxjs/toolkit'
  import additionalMiddleware from 'additional-middleware'
  import logger from 'redux-logger'
  // @ts-ignore
  import untypedMiddleware from 'untyped-middleware'
  import rootReducer from './rootReducer'

  type RootState = ReturnType<typeof rootReducer>
  const store = configureStore({
    reducer: rootReducer,
    middleware: getDefaultMiddleware =>
      getDefaultMiddleware()
        .prepend(
          // correctly typed middlewares can just be used
          additionalMiddleware,
          // you can also type middlewares manually
          untypedMiddleware as Middleware<
            (action: Action<'specialAction'>) => number,
            RootState
          >
        )
        // prepend and concat calls can be chained
        .concat(logger)
  })

  type AppDispatch = typeof store.dispatch
  ```

</details>
<details>
  <summary>How is the type of the `dispatch` function inferred?</summary>
  <br/>

  The type of the `dispatch` function type will be directly inferred from the middleware option. So if you add _correctly types_ middlewares, `dispatch` should already be correctly typed.

</details>
<details>
  <summary>Why is the spread operator not a good choice for combining arrays for the `configureStore` `middleware` option and what is the alternative?</summary>
  <br/>

  As TypeScript often widens array types when combining arrays using the spread operator, we suggest using the `.concat(...)` and `.prepend(...)` methods of the `MiddlewareArray` returned by `getDefaultMiddleware()`.

  ```ts
  import { configureStore } from '@reduxjs/toolkit'
  import additionalMiddleware from 'additional-middleware'
  import logger from 'redux-logger'
  // @ts-ignore
  import untypedMiddleware from 'untyped-middleware'
  import rootReducer from './rootReducer'

  type RootState = ReturnType<typeof rootReducer>
  const store = configureStore({
    reducer: rootReducer,
    middleware: getDefaultMiddleware =>
      getDefaultMiddleware()
        .prepend(
          // correctly typed middlewares can just be used
          additionalMiddleware,
          // you can also type middlewares manually
          untypedMiddleware as Middleware<
            (action: Action<'specialAction'>) => number,
            RootState
          >
        )
        // prepend and concat calls can be chained
        .concat(logger)
  })

  type AppDispatch = typeof store.dispatch
  ```

</details>
<details>
  <summary>Which is the type-safe way of using `createReducer`?</summary>
  <br/>

  The default way of calling `createReducer` would be with a "lookup table" / "map object", like this:

  ```ts
  createReducer(0, {
    increment: (state, action: PayloadAction<number>) => state + action.payload
  })
  ```

  Unfortunately, as the keys are only strings, using that API TypeScript can neither infer not validate the action types for you:

  ```ts
  {
    const increment = createAction<number, 'increment'>('increment')
    const decrement = createAction<number, 'decrement'>('decrement')
    createReducer(0, {
      [increment.type]: (state, action) => {
        // action is any here
      },
      [decrement.type]: (state, action: PayloadAction<string>) => {
        // even though action should actually be PayloadAction<number>, TypeScript can't detect that and won't give a warning here.
      }
    })
  }
  ```

  As an alternative, RTK includes a type-safe reducer builder API:

  ```ts
  const increment = createAction<number, 'increment'>('increment')
  const decrement = createAction<number, 'decrement'>('decrement')
  createReducer(0, builder =>
    builder
      .addCase(increment, (state, action) => {
        // action is inferred correctly here
      })
      .addCase(decrement, (state, action: PayloadAction<string>) => {
        // this would error out
      })
  )
  ```

</details>
<details>
  <summary>How can Action types be provided to `createSlice`?</summary>
  <br/>

  As `createSlice` creates your actions as well as your reducer for you, you don't have to worry about type safety here. Action types can just be provided inline:

  ```ts
  {
    const slice = createSlice({
      name: 'test',
      initialState: 0,
      reducers: {
        increment: (state, action: PayloadAction<number>) =>
          state + action.payload
      }
    })
    // now available:
    slice.actions.increment(2)
    // also available:
    slice.caseReducers.increment(0, { type: 'increment', payload: 5 })
  }
  ```

  If you have too many reducers and defining them inline would be messy, you can also define them outside the `createSlice` call and type them as `CaseReducer`:

  ```ts
  type State = number
  const increment: CaseReducer<State, PayloadAction<number>> = (state, action) =>
    state + action.payload

  createSlice({
    name: 'test',
    initialState: 0,
    reducers: {
      increment
    }
  })
  ```

</details>
<details>
  <summary>How can you define the Initial State Type on `createSlice`?</summary>
  <br/>

  The standard approach is to declare an interface or type for your state, create an initial state value that uses that type, and pass the initial state value to `createSlice`. You can also use the construct `initialState: myInitialState as StateSlice`.

  ```ts
  type SliceState = { state: 'loading' } | { state: 'finished'; data: string }

  // First approach: define the initial state using that type
  const initialState: SliceState = { state: 'loading' }

  createSlice({
    name: 'test1',
    initialState, // type SliceState is inferred for the state of the slice
    reducers: {}
  })

  // Or, cast the initial state as necessary
  createSlice({
    name: 'test2',
    initialState: { state: 'loading' } as SliceState,
    reducers: {}
  })
  ```

</details>
<details>
  <summary>Why it is not a good idea to pass `SliceState` type as a generic to `createSlice`?</summary>
  <br/>

  This is due to the fact that in almost all cases, follow-up generic parameters to `createSlice` need to be inferred, and TypeScript cannot mix explicit declaration and inference of generic types within the same "generic block".

</details>
<details>
  <summary>How is the Action Content defined with the `prepare` callback in `createSlice`?</summary>
  <br/>

  ```ts
  const blogSlice = createSlice({
    name: 'blogData',
    initialState,
    reducers: {
      receivedAll: {
        reducer(
          state,
          action: PayloadAction<Page[], string, { currentPage: number }>
        ) {
          state.all = action.payload
          state.meta = action.meta
        },
        prepare(payload: Page[], currentPage: number) {
          return { payload, meta: { currentPage } }
        }
      }
    }
  })
  ```

</details>
<details>
  <summary>How can type safety be accomplished in the `extraReducers` option of `createSlice`?</summary>
  <br/>

  Like with `createReducer`, you may also use the "builder callback" approach for defining the reducer object argument.

</details>\
<details>
  <summary>Do you need to explicitly declare any types for the `createAsyncThunk` call itself?</summary>
  <br/>

  In the most common use cases, you should not need to explicitly declare any types for the `createAsyncThunk` call itself.

  Just provide a type for the first argument to the `payloadCreator` argument as you would for any function argument, and the resulting thunk will accept the same type as its input parameter. The return type of the `payloadCreator` will also be reflected in all generated action types.

  ```ts
  interface MyData {
    // ...
  }

  const fetchUserById = createAsyncThunk(
    'users/fetchById',
    // Declare the type your function argument here:
    async (userId: number) => {
      const response = await fetch(`https://reqres.in/api/users/${userId}`)
      // Inferred return type: Promise<MyData>
      return (await response.json()) as MyData
    }
  )

  // the parameter of `fetchUserById` is automatically inferred to `number` here
  // and dispatching the resulting thunkAction will return a Promise of a correctly
  // typed "fulfilled" or "rejected" action.
  const lastReturnedAction = await store.dispatch(fetchUserById(3))
  ```

    ```ts
  interface MyData {
    // ...
  }

  const fetchUserById = createAsyncThunk<MyData>(
    'users/fetchById',
    // Declare the type your function argument here:
    async (userId: number) => {
      const response = await fetch(`https://reqres.in/api/users/${userId}`)
      // Inferred return type: Promise<MyData>
      return (await response.json())
    }
  )

  // the parameter of `fetchUserById` is automatically inferred to `number` here
  // and dispatching the resulting thunkAction will return a Promise of a correctly
  // typed "fulfilled" or "rejected" action.
  const lastReturnedAction = await store.dispatch(fetchUserById(3))
  ```

</details>
<details>
  <summary>What type does `createEntityAdapter` requires you to specify?</summary>
  <br/>

  Typing `createEntityAdapter` only requires you to specify the entity type as the single generic argument.

  ```ts
  interface Book {
    bookId: number
    title: string
    // ...
  }

  const booksAdapter = createEntityAdapter<Book>({
    selectId: book => book.bookId,
    sortComparer: (a, b) => a.title.localeCompare(b.title)
  })

  const booksSlice = createSlice({
    name: 'books',
    initialState: booksAdapter.getInitialState(),
    reducers: {
      bookAdded: booksAdapter.addOne,
      booksReceived(state, action: PayloadAction<{ books: Book[] }>) {
        booksAdapter.setAll(state, action.payload.books)
      }
    }
  })
  ```

</details>