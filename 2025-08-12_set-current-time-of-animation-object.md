---
date: 2025-08-12
type: fact
aliases:
  -
hubs:
  - "[web-animation](./web-animation.md)"
---

# set current time of animation object

```js
document.addEventListener("DOMContentLoaded", () => {
  const element = document.querySelector(".square");
  const squareAnimation = element.animate(
    [ ... ],
    {
      duration: 3000,
      delay: 1000,
      direction: "alternate",
      fill: "both",
      iterations: 2,
      easing: "linear",
      composite: "add",
    }
  );
  squareAnimation.pause();

  const buttons = document.querySelectorAll(".button");
  buttons.forEach((button) => { ... });

  // ...
});
```

We can visualize the above animation using the following image:


![current-time-visualization.png](../assets/imgs/current-time-visualization.png)
By default, the `currentTime` of the animation object is set to `0`, when the animation starts, the `currentTime` will increase until it reaches the end of the animation.

But we can set the `currentTime` of the animation to a specific value, so when we play the animation, it will start from that specific time instead of `0`.

```js
document.addEventListener("DOMContentLoaded", () => {
  const element = document.querySelector(".square");
  const squareAnimation = element.animate(
    [ ... ],
    {
      duration: 3000,
      delay: 1000,
      direction: "alternate",
      fill: "both",
      iterations: 2,
      easing: "linear",
      composite: "add",
    }
  );
  squareAnimation.pause();

  const buttons = document.querySelectorAll(".button");
  buttons.forEach((button) => { ... });

  // ...

  squareAnimation.currentTime = 1000; // Set the current time to 1000 milliseconds initially, so when we play the animation, it will skip the delay and start immediately.
  // squareAnimation.currentTime = squareAnimation.effect.getTiming().delay; // instead of hardcoding the value, we can get the delay from the effect's timing object.
});
```


```js
document.addEventListener("DOMContentLoaded", () => {
  const element = document.querySelector(".square");
  const squareAnimation = element.animate(
    [ ... ],
    {
      duration: 3000,
      delay: 1000,
      direction: "alternate",
      fill: "both",
      iterations: 2,
      easing: "linear",
      composite: "add",
    }
  );
  squareAnimation.pause();

  const buttons = document.querySelectorAll(".button");
  buttons.forEach((button) => { ... });

  // ...

  squareAnimation.currentTime = 4000; // Set the current time to 4000 milliseconds initially, which is the end of the first iteration of the animation. So when we play the animation, it will start from the end of the first iteration and directly go to the second iteration.

  // instead of hardcoding the value, we can get the end time of the first iteration from the effect's computed timing object as follows:
  // squareAnimation.currentTime =
  //   squareAnimation.effect.getComputedTiming().activeDuration / 2 +
  //   squareAnimation.effect.getTiming().delay;
});
```


```js
document.addEventListener("DOMContentLoaded", () => {
  const element = document.querySelector(".square");
  const squareAnimation = element.animate(
    [ ... ],
    {
      duration: 3000,
      delay: 1000,
      direction: "alternate",
      fill: "both",
      iterations: 2,
      easing: "linear",
      composite: "add",
    }
  );
  squareAnimation.pause();

  const buttons = document.querySelectorAll(".button");
  buttons.forEach((button) => { ... });

  // ...

  squareAnimation.currentTime = 7000; // Set the current time to 7000 milliseconds initially, which is the end of the entire animation. So when we play the animation, it will start a new animation from the beginning.
  // squareAnimation.currentTime = squareAnimation.effect.getComputedTiming().endTime; // instead of hardcoding the value, we can get the end time of the entire animation from the effect's computed timing object
});
```

We can create a input on the page for the user to set the `currentTime` of the animation object.

```html
<!DOCTYPE html>
<html lang="en">
  ...
  <body>
    <div class="square"></div>
    <div class="controls">
      <button class="button logInfo">Log Info</button>
      <div class="section">
        ...
        <br /><br />
        <label for="currentTimeInput">Current Time</label>
        <input type="number" id="currentTimeInput" step="500" min="0" /> <!-- input for setting the current time of the animation -->
      </div>
      <div class="section">
        ...
      </div>
    </div>
  </body>
</html>
```

```js
document.addEventListener("DOMContentLoaded", () => {
  const element = document.querySelector(".square");
  const squareAnimation = element.animate( ... );
  squareAnimation.pause();

  const buttons = document.querySelectorAll(".button");
  buttons.forEach((button) => { ... });

  //...

  const currentTimeInput = document.getElementById("currentTimeInput"); // get the input element
  currentTimeInput.value = squareAnimation.currentTime; // set the initial value of the input to the current time
  // update the input value when the current time input changes
  currentTimeInput.addEventListener("input", (e) => {
    squareAnimation.currentTime = e.target.value;
  });
});
```
