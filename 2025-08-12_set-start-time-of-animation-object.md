# set start time of animation object

## Set start time of animation object

```js
document.addEventListener("DOMContentLoaded", () => {
  const element = document.querySelector(".square");
  const squareAnimation = element.animate( ... );
  squareAnimation.pause();

  const buttons = document.querySelectorAll(".button");
  buttons.forEach((button) => { ... });

  //...

  squareAnimation.startTime = 3000;
});
```

![3s-start-time.png](../assets/imgs/3s-start-time.png)

In the example above, we actively set the `startTime` property of the animation object to 3000 milliseconds, which means the animation will start playing at the 3-second mark on the timeline automatically. (With an additional delay of 1 second for the animation itself, we will see the animation start playing 4 seconds after the page loads)

## Negative start time

We can also set `startTime` to a negative value.

```js
document.addEventListener("DOMContentLoaded", () => {
  const element = document.querySelector(".square");
  const squareAnimation = element.animate( ... );
  squareAnimation.pause();

  const buttons = document.querySelectorAll(".button");
  buttons.forEach((button) => { ... });

  //...

  squareAnimation.startTime = -1000;
});
```

![neg-1s-start-time.png](../assets/imgs/neg-1s-start-time.png)

In the example above, startTime is set to a negative value, which happens to be the same as the animation's delay duration. This means that when the page loads, the animation will start playing immediately (skipping the delay time).

```js
document.addEventListener("DOMContentLoaded", () => {
  const element = document.querySelector(".square");
  const squareAnimation = element.animate( ... );
  squareAnimation.pause();

  const buttons = document.querySelectorAll(".button");
  buttons.forEach((button) => { ... });

  //...

  squareAnimation.startTime = -5000;
});
```

![neg-5s-start-time.png](../assets/imgs/neg-5s-start-time.png)
In the example above, we set startTime to a negative value, and the length is just the delay of the animation plus the length of 1 iteration. This means that when the page loads, the animation will directly play the content of the 2nd iteration, and there will be no delay.

## input for startTime

Of course, we can also add an input on the page for users to directly set the startTime.

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
        <label for="startTimeInput">Start Time</label>
        <input type="number" id="startTimeInput" step="500" min="0" />
      </div>
      <div class="section"> ... </div>
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

  // ...

  const startTimeInput = document.getElementById("startTimeInput"); // get the input element
  startTimeInput.value = squareAnimation.startTime; // initialize the input value with the current startTime
                                                    // by default, it will be null, which means the animation will not start automatically
  // update the startTime when the input value changes
  startTimeInput.addEventListener("input", (e) => {
    squareAnimation.startTime = e.target.value;
  });
});
```

> It is important to note that the time set for the start time is a point in time on the timeline. After the page is loaded, the points on the timeline will change over time. Therefore, if we adjust the start time to 1000 after loading the page for some time, it is likely that the animation will not respond because the point on the timeline has already exceeded 1000 milliseconds. Since the start time is a point on the timeline, it will have no effect at this time.
