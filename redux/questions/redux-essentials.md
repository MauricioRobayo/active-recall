# Redux

- [x] https://redux.js.org/tutorials/essentials/part-1-overview-concepts
- [x] https://redux.js.org/tutorials/essentials/part-2-app-structure
- [x] https://redux.js.org/tutorials/essentials/part-3-data-flow
- [x] https://redux.js.org/tutorials/essentials/part-4-using-data
- [x] https://redux.js.org/tutorials/essentials/part-5-async-logic
- [ ] https://redux.js.org/tutorials/essentials/part-6-performance-normalization

---

<details>
  <summary>What is Redux?</summary>
  <br/>

  A way to manage the 'global' state that is used across different parts of your application.
  It is a pattern and a library for managing and updating application state, using events called 'actions'.

</details>
<details>
  <summary>When is Redux more useful?</summary>
  <br/>

  - You have large amounts of application state that are needed in many places in the app.
  - The app state is updated frequently over time.
  - The logic to update the state may be complex.

</details>
<details>
  <summary>How many ways are there to change the state?</summary>
  <br/>

  Only one way, to emit an `action`.

</details>
<details>
  <summary>Where is the whole state of an app stored?</summary>
  <br/>

  It is stored in an object tree inside a single `store`.

</details>
<details>
  <summary>How do you specify how to transform the state tree?</summary>
  <br/>

  By writing pure reducers.

</details>

## Actions

<details>
  <summary>What is an action?</summary>
  <br/>

  An action is a JavaScript object that describes what changed.

</details>
<details>
  <summary>What does an action describes?</summary>
  <br/>

  It describes a change is the state of the app.

</details>
<details>
  <summary>How many fields can an action have?</summary>
  <br/>

  At least, it should have the `type` field. It can have other fields besides that, by convention, the `payload` field is used to store the changes that need to be applied to the state.

</details>
<details>
  <summary>How does usually the `type` field of an actions looks like?</summary>
  <br/>
  
  The `type` field should be a string that gives the _action_ a descriptive name, like "todos/todoAdded".

  We usually write the `type` string like "domain/eventName", where the first part is the feature or category that this actions belongs to, and the second part is the specific thing that happened.

</details>
<details>
  <summary>How does a typical action object looks like?</summary>
  <br/>
  
  ```js
  {
    type: 'todos/todoAdded',
    payload: 'Buy milk',
  }  
  ```

</details>
<details>
  <summary>What is an `action creator`?</summary>
  <br/>

  It is a JavaScript function that creates and returns an action object.

</details>
<details>
  <summary>How does an action creator typically looks like?</summary>
  <br/>

  ```js
  function addTodo(newTodo) {
    return {
      type: 'todos/todoAdded',
      payload: newTodo,
    }
  }
  ```

</details>

## Reducers

<details>
  <summary>What is a `reducer`?</summary>
  <br/>

  It is a pure JavaScript function.

</details>
<details>
  <summary>What are the two responsibilities of a `reducer`?</summary>
  <br/>

  1. Decide how to update the state if necessary.
  2. Return the new state.

</details>

<details>
  <summary>What are the two arguments a `reducer` receives?</summary>
  <br/>

  1. The current state.
  2. An `action` object.

</details>
<details>
  <summary>Why are `reducers` called 'reducers'?</summary>
  <br/>

  Because they look like the callback passed to the `array.reduce()` method:

  ```js
  (state, action) => newState
  ```

</details>
<details>
  <summary>What are the three rules reducers must always follow?</summary>
  <br/>

  1. They should only calculate the new state value based on the `state` and `action` arguments.
  2. They are not allowed to modify existing state. Instead, they must make _immutable updates_ by copying the existing state and making changes to the copied values.
  2. They must not do any asynchronous logic, calculate random values, or cause side effects.

</details>
<details>
  <summary>What are the steps a reducer function usually follows?</summary>
  <br/>

  1. Check to see if the reducer cares about this actions.
  
  If the reducer cares about this action:
  
  2. Make a copy of the state.
  3. Update the copy with new values.
  4. Return the new state.

  Otherwise:

  2. Return the existing state unchanged.
  
</details>
<details>
  <summary>How does a reducer typically looks like?</summary>
  <br/>

  ```javascript
  const initialState = {
    value: 0,
  }

  function addNote(state = initialState, action) {
    // check to see if the reducer cares about this action
    switch (action.type) {
      case 'counter/increment':
        // make a copy of the action
        return {
          ...state,
          // update the copy with the new value
          value: state.value + action.payload
        }
      default:
        // otherwise return the existing state unchanged
        return state;
    }
  }
  ```

</details>
<details>
  <summary>What does `combineReducers` do?</summary>
  <br/>

  It generates a function that calls the reducers with the slices of state selected according to their keys, and combines their results into a single object again.

  ```js
  import { combineReducers } from 'redux'
  import todos from './todos'
  import visibilityFilter from './visibilityFilter'

  export default combineReducers({
    todos,
    visibilityFilter
  })
  ```

  It is basically a shortcut for something like this:

  ```js
  import todos from './todos'
  import visibilityFilter from './visibilityFilter'

  function todoApp(state = {}, action) {
    return {
      visibilityFilter: visibilityFilter(state.visibilityFilter, action),
      todos: todos(state.todos, action)
    }
  }
  ```

</details>

## Store

<details>
  <summary>Where does the current Redux application state live?</summary>
  <br/>

  In an object called `store`.

</details>
<details>
  <summary>How do you create a store?</summary>
  <br/>

  - With plain Redux by using the `createStore` method.
  - With Redux-Toolkit by using the `configureStore` method.

</details>
<details>
  <summary>How do you get the state of an app?</summary>
  <br/>

  By calling the `getState` method which returns the current state.

</details>
<details>
  <summary>How do you update the state in the store?</summary>
  <br/>

  By calling the store `dispatch` method and passing it an action.

</details>
<details>
  <summary>How do you typically dispatch an action?</summary>
  <br/>

  By calling action creators to dispatch the right action.
  ```js
  console.log(store.getState())
  // { value: 1 }

  // action creator
  function increment(value) {
    return {
      type: 'counter/increment',
      payload: value,
    }
  }

  store.dispatch(increment(5))

  console.log(store.getState())
  // { value: 6 }
  ```

</details>

## Selectors

<details>
  <summary>What are selectors?</summary>
  <br/>

  Functions that know which part of the state to extract from the store state.
  
  ```js
  // selector
  const selectCounterValues = (state) => state.value

  // use the selector to get the value from the store state
  const currentValue = selectCounterValue(store.getState())

  console.log(currentValue)
  // { value: 2 }
  ```

</details>

## Data flow

<details>
  <summary>Describe the "one-way data flow".</summary>
  <br/>

  1. State describes the condition fo the app at a point in time, and UI renders based on that state.
  2. When something happens in the app, the state is updated based on what occurred:
      1. The UI dispatches an action.
      2. The store runs the reducers, and the state is updated based on what occurred.
      3. The store notifies the UI that the state has changed.
  3. The UI re-renders based on the new state.

  ![Redux one-way data flow](https://redux.js.org/img/tutorials/essentials/one-way-data-flow.png)

</details>
<details>
  <summary>What are the two main steps in a Redux app data flow?</summary>
  <br/>

  1. Initial setup.
  2. Updates.

</details>
<details>
  <summary>Describe the Redux initial setup.</summary>
  <br/>

  1. A Redux store is created using a root reducer function.
  2. The store call the root reducer once, and saves the return value as its initial `state`.
  3. When the UI is first rendered, UI components access the current state of the Redux store, and use that data to decide what to render. They also subscribe to any future store updates so they can know if the state has changed.

</details>
<details>
  <summary>Describe the Redux updates step.</summary>
  <br/>

  1. Something happens in the app, such as a user clicking a button.
  2. The app code dispatches an action to the Redux store, like `dispatch({type: 'counter/increment'})`.
  3. The store runs the reducer function again with the previous `state` and the current `action`, and saves the returned value as the new `state`.
  4. The store notifies all parts of the UI that are subscribed that the store has been updated.
  5. Each UI component that needs data from the store checks to see if the parts of the state they need have changed.
  6. Each component that sees its data has changed forces a re-render with the new data, so it can update what's shown on the screen.
  
  ![Redux updates](https://redux.js.org/img/tutorials/essentials/ReduxDataFlowDiagram.gif)

</details>
<details>
  <summary>How can we read data from the store inside a component?</summary>
  <br/>

  With the `useSelector` hook from the React-Redux library.
  The 'selector functions' that you write will be called with the entire Redux `state` object as a parameter, and should return the specific data that this component needs from the store.

</details>
<details>
  <summary>How does React know when to update data?</summary>
  <br/>

  Selectors will re-run whenever the Redux store is updated, and if the data they return has changed, the component will re-render.

</details>

## Thunks

<details>
  <summary>What are thunks in Redux?</summary>
  <br/>

  They are an specific kind of Redux function that can contain asynchronous logic.

</details>
<details>
  <summary>How are thunks written?</summary>
  <br/>

  1. An inside thunk function, which gets `dispatch` and `getState` as arguments.
  2. The outside creator function, which creates and returns the thunk function.

</details>
<details>
  <summary>How are thunks useful?</summary>
  <br/>

  A component should not care whether we are dispatching a normal action or starting some async logic.

</details>
<details>
  <summary>What's the typical pattern for data fetching logic that Redux follows?</summary>
  <br/>

  1. A "start" action is dispatch before the request, to indicate that the request is in progress. This may be used to track loading state to allow skipping duplicate requests or show loading indicators in the UI.
  2. The async request is made.
  3. Depending on the request result, the async logic dispatches either a "success" action containing the result data, or a "failure" action containing error details. The reducer logic clears the loading state in both cases, and either processes the result data from the success case, or stores the error value for potential display.
  
  These steps are not _required_, but are commonly used. If all you care about is a successful result, you can just dispatch a single "success" action when the request finishes, and skip "start" and "failure" actions.

</details>

