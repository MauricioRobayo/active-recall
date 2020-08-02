# Redux Thunk

https://github.com/reduxjs/redux-thunk#why-do-i-need-this

<details>
  <summary>What's the most common use case for Redux Thunk?</summary>
  <br/>

  Inside of a thunk function, you can write any code you want. The most common usage is fetching some data via an AJAX call, and dispatching an action to load that data into the Redux store. The `async/await` syntax makes it easier to write thunks that do AJAX calls.

</details>
<details>
  <summary>What does Redux Thunk enable us to do?</summary>
  <br/>

  Adding a thunk middleware to our Redux store lets us pass functions directly to `store.dispatch()`. The thunk middleware will see the function, prevent it from actually reaching the "real" store, and call our function and pass in `dispatch` and `getState` as arguments.

</details>
<details>
  <summary>Why use Thunks?</summary>
  <br/>

  1. Thunks allow us to write reusable logic that interacts with a Redux store, but without needing to reference a specific store instance.
  2. Thunks enable us to move more complex logic outside of our components.
  3. Fom a component's point of view, it doesn't care whether it's dispatching a plain action or kicking off some async logic - it just calls `dispatch(doSomething())` and moves on.
  4. Thunks can return values like promises, allowing logic inside the component to wait for something else to finish.

</details>
