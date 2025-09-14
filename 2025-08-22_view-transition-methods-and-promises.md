---
date: 2025-08-22
type: fact
aliases:
  -
hubs:
  - "[web-animation](./web-animation.md)"
---

# view transition methods and promises

```js
document.addEventListener("DOMContentLoaded", () => {
  // ...

  function expandImage(item) { ... }

  function displayGrid() { ... }

  grid.addEventListener("click", async (e) => {
    // ...

    if (!document.startViewTransition) {
      expandImage(item);
      return;
    }

    document.startViewTransition(() => {
      expandImage(item);
    });
  });
  
  gridButton.addEventListener("click", async (e) => {

    if (!document.startViewTransition) {
      displayGrid();
      return;
    }
    document.startViewTransition(() => {
      displayGrid();
    });
  });
});
```


```js
document.addEventListener("DOMContentLoaded", () => {
  // ...

  grid.addEventListener("click", async (e) => {
    // ...

    // when we use `document.startViewTransition`, it returns a `ViewTransition` object
    // which has several methods and promises that we can use to track the transition state
    const transition = document.startViewTransition(() => {
      expandImage(item);
    });

    try {
      await transition.ready;
      console.log(
        "transition.ready pseudo-element tree is created and the transition animation is about to start"
      );
    } catch (error) {
      console.error("transition.ready error:", error);
    }

    try {
      await transition.updateCallbackDone;
      console.log(
        "transition.updateCallbackDone DOM was updated successfully but that dose not guarantee that the transition animation was successful."
      );
    } catch (error) {
      console.error("transition.updateCallbackDone error:", error);
    }

    try {
      await transition.finished;
      console.log(
        "transition.finished transition animation is finished, and the new page view is visible and interactive to the user."
      );
    } catch (error) {
      console.error("transition.finished error:", error);
    }
  });
  
  // ...
});
```

## Difference between `updateCallbackDone` and `finished`

You may think that `updateCallbackDone` and `finished` are very similar promises, but `updateCallbackDone` will only fulfill once the callback function of `document.startViewTransition` is completed, while `finished` will fulfill only after the transition animation is finished. The difference between the two will be more obvious when the animation duration is longer.

## How about we use duplicated view transition name?

If we use a duplicate `view-transition-name` in CSS, and trigger the related transition in the browser, the console will display the following:

```
script.js:57 transition.ready error: InvalidStateError: Transition was aborted because of invalid state
(anonymous) @ script.js:57Understand this error

script.js:62 transition.updateCallbackDone DOM was updated successfully but that dose not guarantee that the transition animation was successful.

script.js:71 transition.finished transition animation is finished, and the new page view is visible and interactive to the user.
```

It can be seen that `transition.ready` is rejected, but the other two promises are resolved. This is because there are duplicate `view-transition-name` causing the inability to obtain the correct new state and unable to create a pseudo-element tree, resulting in `transition.ready` being rejected. However, the callback still updates the DOM, so `transition.updateCallbackDone` is still resolved. And because the DOM update is completed, `transition.finished` is also resolved even the transition is not started.

## So... when the other two promises are rejected?

If any error occurs in the callback function of `document.startViewTransition` as shown below:

```js
    const transition = document.startViewTransition(() => {
      throw new Error("error") // simulate an error in the callback
      expandImage(item);
    });
```

Then we trigger the transition on the browser, the console will display the following:

```
script.js:58 transition.ready error: Error: error
    at script.js:48:13
(anonymous) @ script.js:58Understand this error
script.js:67 transition.updateCallbackDone error: Error: error
    at script.js:48:13
(anonymous) @ script.js:67Understand this error
script.js:76 transition.finished error: Error: error
    at script.js:48:13

```

All three promises will be rejected, because the callback function throws an error, which prevents the DOM from being updated and the transition from being completed.

## Method to skip the transition

ViewTransition also provides a method to skip the transition, which is `skipTransition()`. This method can be used when you want to update the DOM immediately without waiting for the transition animation to complete.

```js

document.addEventListener("DOMContentLoaded", () => {
  // ...

  grid.addEventListener("click", async (e) => {
    // ...

    const transition = document.startViewTransition(() => {
      expandImage(item);
    });

    transition.skipTransition(); // skip the transition and immediately update the DOM
                                 // so the callback of `startViewTransition` still runs
    // ...
});
```


Let's trigger the transition in the browser, the console will display the following:


```
script.js:59 transition.ready error: AbortError: Transition was skipped
(anonymous) @ script.js:59Understand this error

script.js:64 transition.updateCallbackDone DOM was updated successfully but that dose not guarantee that the transition animation was successful.

script.js:73 transition.finished transition animation is finished, and the new page view is visible and interactive to the user.
```

As you can see, `transition.ready` is rejected with an `AbortError`, indicating that the transition was skipped. However, the other two promises are still resolved, because the callback function of `startViewTransition` is still executed and the DOM is updated successfully.


