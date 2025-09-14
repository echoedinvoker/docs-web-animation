# Persisting Filling Animations Styles With CommitStyles

When we use the `fill` property with `both` or `forwards` value in an animation, the styles of the last frame will be persisted by the browser, not the element. This means that the browser consumes memory for these styles, and they have a higher priority than the styles on the element itself. This can make it difficult to override these styles later. Let's see an example of this behavior.

```js
document.addEventListener("DOMContentLoaded", async () => {
  const element = document.querySelector(".square");

  const squareAnimation = element.animate(
    [
      {
        transform: "translateX(0)",
        easing: "ease-in",
      },
      {
        backgroundColor: "blue",
        offset: 0.8,
        composite: "replace",
      },

      // the styles of the last frame will be persisted to the element
      {
        transform: "translateX(calc(100vw - 100px)) rotate(360deg)",
        backgroundColor: "crimson",
      },
    ],
    {
      duration: 3000,
      delay: 1000,
      direction: "alternate",
      fill: "both", // or "forwards" to persist the styles of the last frame to the element
      iterations: 1,
      easing: "linear",
      composite: "replace",
      iterationComposite: "accumulate",
      timeline: document.timeline,
    }
  );

  const buttons = document.querySelectorAll(".button");
  buttons.forEach((button) => { ... });
  // ...
});
```


```
animation style { <-- this styles are persisted by the browser, not element, so it consume the memory
    background-color: rgb(220, 20, 60);
    transform: translateX(566px) rotate(360deg);
}
element.style {
}
```

```js
document.addEventListener("DOMContentLoaded", async () => {
  const element = document.querySelector(".square");

  const squareAnimation = element.animate(
    [
      { ... },
      { ... },

      // with fill: "both" or "forwards", the styles of the last frame will be persisted by the browser
      // it has higher priority than the styles on the element, so it's hard to be overridden
      {
        transform: "translateX(calc(100vw - 100px)) rotate(360deg)",
        backgroundColor: "crimson",
      },
    ],
    {
      duration: 3000,
      delay: 1000,
      direction: "alternate",
      fill: "both",
      iterations: 1,
      easing: "linear",
      composite: "replace", // change to "replace"
      iterationComposite: "accumulate",
      timeline: document.timeline,
    }
  );

  // ...

  squareAnimation.addEventListener("finish", () => {
    element.style.backgroundColor = "green"; // this won't override the animation styles
  })
});
```

```
animation style {
    background-color: rgb(220, 20, 60); <-- have higher priority than element.style
    transform: translateX(566px) rotate(360deg);
}
element.style {
    background-color: green; <-- this won't override the animation styles
}
```

So the last frame styles of the animation are persisted by the browser, but it consumes memory and hard to override. We can use `commitStyles()` to persist the last frame styles to the element from the computed styles of the animation object.

```js
  squareAnimation.addEventListener("finish", () => {
    squareAnimation.commitStyles();
  });
```


```
animation style {
    background-color: rgb(220, 20, 60);
    transform: translateX(566px) rotate(360deg);
}
element.style { <-- the last frame styles are copy pasted to the element from computed styles of animation object
    background-color: rgb(220, 20, 60);
    transform: translateX(566px) rotate(360deg);
}
```

Even though last frame styles have been persisted to the element, the animation styles still exist. We must use `cancel()` to cancel the animation in order to remove the animation styles. Otherwise, they will continue to exist in the browser memory until the page is unloaded.

```js
  squareAnimation.addEventListener("finish", () => {
    squareAnimation.commitStyles();
    squareAnimation.cancel(); // cancel the animation to remove the animation styles
  });
```


```
// animation styles are removed

element.style {  <-- but last frame styles are persisted to the element, so it's fine
    background-color: rgb(220, 20, 60);
    transform: translateX(566px) rotate(360deg);
}
```
