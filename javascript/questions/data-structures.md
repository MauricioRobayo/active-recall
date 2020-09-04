# Data Structures

<details>
  <summary>Regarding the order of the keys, what's the difference between a Hash (Object) and a Map?</summary>
  <br/>

  The key in `Map` is ordered. Thus, when iterating over it, a `Map` object returns keys in order of insertion.

  The keys of an `Object` are note ordered.

  **Note**: Since ECMAScript 2015, objects _do_ preserve creation order for string and `Symbol` keys. In JavaScript engines that comply with the ECMAScript 2015 spec, iterating over an object with only string keys will yield the keys in order of insertion.

</details>