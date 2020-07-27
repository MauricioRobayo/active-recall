# Redux

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

## Redux Toolkit
