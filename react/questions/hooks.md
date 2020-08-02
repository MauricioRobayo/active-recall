# Hooks

## `useEffect`

<details>
  <summary>Why we can't declare `useEffect` callback as `async`?</summary>
  <br/>

  React expects that the return value from `useEffect` callback will be a cleanup function. Since all async functions return a `Promise` automatically, React would see that `Promise` instead, and that would prevent React from actually cleaning up correctly.

</details>