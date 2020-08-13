# Intersection Observer API

<details>
  <summary>What does the Intersection Observer API provides us?</summary>
  <br/>

  A way to asynchronously observe changes in the intersection of a target element with an ancestor element or with a top-level document's viewport.

</details>
<details>
  <summary>Name three scenarios in which the Intersection Observer API is useful?</summary>
  <br/>

  - Lazy-loading of images or another content as a page is scrolled.
  - Implementing "infinite scrolling" web sites, where more and more content is loaded and rendered as you scroll, so that the user doesn't have to flip through pages.
  - Reporting of visibility of advertisements in order to calculate ad revenues.
  - Deciding whether or not to perform tasks or animation processes based on whether or not the user will see the result.

</details>
<details>
  <summary>Question goes here</summary>
  <br/>

  This is the hidden answer

</details>
<details>
  <summary>What problem does the Intersection Observer API solves?</summary>
  <br/>

  Implementing intersection detection in the past involved event handlers and loops. Since all this code runs on the main thread, even one of these can cause performance problems.

  > Consider a web page that uses infinite scrolling. It uses a vendor-provided library to manage the advertisements placed periodically throughout the page, has animated graphics here and there, and uses a custom library that draws notification boxes and the like. Each of these has its own intersection detection routines, all running on the main thread. The author of the web site may not even realize this is happening, since they may know very little about the inner workings of the two libraries they are using. As the user scrolls the page, these intersection detection routines are firing constantly during the scroll handling code, resulting in an experience that leaves the user frustrated with the browser, the web site, and their computer.

  https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API

</details>
<details>
  <summary>How does the Intersection Observer API works?</summary>
  <br/>

  It lets code register a callback function that is executed whenever an element they wish to monitor enters or exits another element (or the viewport), or when the amount by which the two intersect changes by a requested amount.

  This way, sites no longer need to do anything on the main thread to watch for this kind of element intersection, and the browser is free to optimize the management of intersections as it sees fit.

</details>
<details>
  <summary>What does the Intersection Observer API tells you?</summary>
  <br/>

  The exact number of pixels that overlap or specifically which ones they are; however, it covers the much more common use case of "if they intersect by somewhere around N%, I need to do something"

</details>
<details>
  <summary>When is the Intersection Observer API callback called?</summary>
  <br/>

  1. Whenever one element, called the **target**, intersects either the device viewport or a specified element; for the purpose of this API, this is called the **root element** or **root**.
  2. Whenever the observer is asked to watch a target for the very first time

</details>
<details>
  <summary>What's the `intersection ratio`?</summary>
  <br/>

  This is a representation of the percentage of the target element which is visible as a value between 0.0 0.1.

</details>
<details>
  <summary>How is an intersection observer created?</summary>
  <br/>

  By calling its constructor and passing it a callback function to be run whenever a threshold is crossed in one direction or the other:

  ```js
  let options = {
    root: document.querySelector('#scrollArea'),
    rootMargin: '0px',
    threshold: 1.0
  }

  let observer = new IntersectionObserver(callback, options);
  ```

</details>
<details>
  <summary>What does a `threshold` of 1.0 means?</summary>
  <br/>

  It means that when 100% of the target is visible within the element specified by the `root` option, the callback is invoked.

</details>
<details>
  <summary>Which are the `options` of the intersection observer constructor?</summary>
  <br/>

  - root
  - rootMargin
  - threshold

</details>
<details>
  <summary>What does the `root` option does?</summary>
  <br/>

  The element that is used as the viewport for checking visibility of the target. Must be the ancestor of the target. Defaults to the browser viewport if not specified or if `null`.

</details>
<details>
  <summary>What does the `rootMargin` option does?</summary>
  <br/>

  This set of values serves to grow or shrink each side of the root element's bounding box before computing intersections. Defaults to all zeros.

  Margin around the root. Can have values similar to CSS `margin` property, e.g. "10px 20px 30px 40px! (top, right, bottom, left). The values can be percentages.

</details>
<details>
  <summary>What does the `threshold` option does?</summary>
  <br/>

  Either a single number or an array of numbers which indicate at what percentage of the target's visibility the observer's callback should be executed. If you only want to detect when visibility passes 50% mark, you can use a value of 0.5. If you want the callback to run every time visibility passes another 25%, you would specify the array [0, 0.25, 0.50, 0.75, 1]. The default is 0 (meaning as soon as even on pixel is visible, the callback will be run). A value of 1.0 means that the threshold isn't considered passed until every pixel is visible.

</details>
<details>
  <summary>How do you target an element to be observed?</summary>
  <br/>

  Once you have created the observer, you need to give it a target element to watch:

  ```js
  let target = document.querySelector('#listItem');
  observer.observe(target);

  // the callback we setup for the observer will be executed now for the first time
  // it waits until we assign a target to our observer (even if the target is currently not visible)
  ```

</details>
<details>
  <summary>What does the callback receives?</summary>
  <br/>

  It gets a list of [`IntersectionObserverEntry`](https://developer.mozilla.org/en-US/docs/Web/API/IntersectionObserverEntry) objects and the observer.

  ```js
  let callback = (entries, observer) => {
    entries.forEach(entry => {
      // Each entry describes an intersection change for one observed
      // target element:
      //   entry.boundingClientRect
      //   entry.intersectionRatio
      //   entry.intersectionRect
      //   entry.isIntersecting
      //   entry.rootBounds
      //   entry.target
      //   entry.time
    });
  };
```

</details>
<details>
  <summary>What does the list of entries received by the callback includes?</summary>
  <br/>

  It includes one entry for each target reporting a change in its intersection status. Check the value of the `isIntersecting` property to see if the entry represents an element that currently intersects with the root.

</details>
<details>
  <summary>Is the callback executed on the main thread or asynchronously?</summary>
  <br/>

  The callback is executed on the main thread. It should operate as quickly as possible; if anything time-consuming needs to be done, use [`Window.requestIdleCallback()`](https://developer.mozilla.org/en-US/docs/Web/API/Window/requestIdleCallback).

</details>