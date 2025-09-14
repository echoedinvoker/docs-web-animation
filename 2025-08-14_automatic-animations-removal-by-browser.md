---
date: 2025-08-14
type: fact
aliases:
  -
hubs:
  - "[web-animation](./web-animation.md)"
---

# automatic animations removal by browser

## Mechanism of automatic removal by the browser

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="stylesheet" href="styles.css" />
    <title>Web Animations API</title>
    <script src="./script.js"></script>
  </head>
  <body>
    <div class="square"></div>

    <div class="controls">
      <br />
      <button id="add-animation-button">Add Animation</button>
      <button id="log-animations-button">Log Animations</button>
      <br />
    </div>
  </body>
</html>
```

```js
document.addEventListener("DOMContentLoaded", async () => {
  const element = document.querySelector(".square");

  const animations = []; // we create an array to store animations we create

  // instead of creating animations directly, we create a function to add animations
  const addAnimation = () => {
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
        {
          transform: "translateX(calc(100vw - 100px)) rotate(360deg)",
          backgroundColor: "crimson",
        },
      ],
      {
        duration: 3000,
        direction: "alternate",
        fill: "both",
        iterations: 1,
        easing: "linear",
        composite: "replace",
        iterationComposite: "accumulate",
        timeline: document.timeline,
      }
    );
    // everytime we create an animation, we add it to the array which stores animations
    animations.push(squareAnimation);
  }

  const addAnimationButton = document.querySelector("#add-animation-button");
  const logAnimationsButton = document.querySelector("#log-animations-button");

  addAnimationButton.addEventListener("click", addAnimation);
  logAnimationsButton.addEventListener("click", () => {
    console.log(animations); // check the animations array
    console.log(element.getAnimations()); // check the animations on the element
  });
});
```

Now we click the "Add Animation" button four times, and then click the "Log Animations" button.


```
[Animation, Animation, Animation, Animation]
[Animation] // only one animation on the element, because the browser automatically removes the replaced animations
```

Browser automatically removes the replaced animations if you create a new animation on the same element. This is done to prevent memory leaks and keep the performance optimal.

`replaceState: "active"` // "removed"


## Event `remove` on Animation

When the animation is removed by the browser, it triggers the `remove` event on the Animation object. This event can be listened to, allowing you to perform actions when an animation is removed.


```js
document.addEventListener("DOMContentLoaded", async () => {
  const element = document.querySelector(".square");
  const animations = [];
  const addAnimation = () => {
    const squareAnimation = element.animate( ... );

    // Listen for the 'remove' event
    squareAnimation.addEventListener("remove", (e) => {
      console.log("Animation removed by the browser", e);
    });

    animations.push(squareAnimation);
  }

  //...
});
```

```
Animation removed by the browser 
    AnimationPlaybackEvent { ... }
Animation removed by the browser 
    AnimationPlaybackEvent { ... }
Animation removed by the browser 
    AnimationPlaybackEvent { ... }
Animation removed by the browser 
    AnimationPlaybackEvent {
        ...
        currentTarget: Animation {
            ...
            replaceState: "removed",
        },
    }
```

## Persisting Animations

If you want to prevent the browser from automatically removing the animations, you can use the `persist()` method on the Animation object. This will keep the animation in memory even if a new animation is created on the same element.

```js
document.addEventListener("DOMContentLoaded", async () => {
  const element = document.querySelector(".square");
  const animations = [];
  const addAnimation = () => {
    const squareAnimation = element.animate( ... );
    squareAnimation.persist(); // Prevent automatic removal by the browser
    squareAnimation.addEventListener("remove", (e) => { ... });
    animations.push(squareAnimation);
  }
  //...
});
```

```
[Animation, Animation, Animation, Animation]
[Animation, Animation, Animation, Animation]  // there are four animations on the element instead of one

// event listeners not triggered because there is no automatic removal
```

```
Animation {
    ...
    replaceState: "persisted",  // all animations are persisted state, even the active ones
}
```

> `persist()` method might lead to memory leaks if not used carefully, as it prevents the browser from cleaning up animations that are no longer needed. Use it only when necessary, such as when you need to keep the animation data for later use or debugging.
