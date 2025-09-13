---
date: 2025-08-11
type: fact
aliases:
  -
hubs:
  - "[[web-animation]]"
---

# buttons to call animation object methods

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
      <div class="section">
        <h3>Animation Object (DVD Player)</h3>
        <!-- below are the buttons to call animation object methods to manipulate the animation of the square -->
        <button class="button play">Play</button>
        <button class="button pause">Pause</button>
        <button class="button cancel">Cancel</button>
        <button class="button reverse">Reverse</button>
        <button class="button finish">Finish</button>
        <br /><br />
        <label for="playbackRateInput">Playback Rate</label>
        <input type="range" id="playbackRateInput" step="0.2" min="0" max="2" />
        <output id="playbackRateInputValue" for="playbackRateInput">0</output>
      </div>
    </div>
  </body>
</html>
```

```js
document.addEventListener("DOMContentLoaded", () => {
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
      {
        transform: "translateX(calc(100vw - 100px)) rotate(360deg)",
        backgroundColor: "crimson",
      },
    ],
    {
      duration: 3000, // this duration belongs to the keyframe effect, not the animation object. You can think of it as the length of the DVD movie
      delay: 1000,
      direction: "alternate",
      fill: "both",
      iterations: Infinity,
      easing: "linear",
      composite: "add",
      iterationComposite: "accumulate",
      timeline: document.timeline,
    }
  );
  squareAnimation.pause();

  // because all the buttons have the same class `button`, we can use querySelectorAll to select all buttons
  // then use `forEach` to iterate over each button and add an event listener to call the respective animation object method
  const buttons = document.querySelectorAll(".button");
  buttons.forEach((button) => {
    button.addEventListener("click", () => { // iterate over each button and add an event listener
      if (button.classList.contains("play")) { // use 2nd class to check which button was clicked
        squareAnimation.play(); // and call the respective method on the animation object
      }
      if (button.classList.contains("pause")) {
        squareAnimation.pause();
      } //             ^^^^^^^^^ stop the playback of the animation
      if (button.classList.contains("cancel")) {
        squareAnimation.cancel();
      } //             ^^^^^^^^^ cancel the animation and the square will return to its initial state
      if (button.classList.contains("reverse")) {
        squareAnimation.reverse();
      } //             ^^^^^^^^^ reverse the playback direction of the animation
      if (button.classList.contains("finish")) {
        squareAnimation.finish();
      } //             ^^^^^^^^^ finish the animation and set the square to its final state
    });
  });

  // Handle playback rate input
  const playbackRateInput = document.getElementById("playbackRateInput");
  const playbackRateInputValue = document.getElementById(
    "playbackRateInputValue"
  );

  // whenever the playback rate input changes, the `input` event is fired
  // and we can update the playback rate of the animation object by `updatePlaybackRate` method
  playbackRateInput.addEventListener("input", (e) => {
    squareAnimation.updatePlaybackRate(e.target.value); // use the `updatePlaybackRate` method of the animation object to change the playback rate
    playbackRateInputValue.value = e.target.value;
  });
});
```

Do not confuse the `playbackRate` property of the animation object with the `duration` property of the keyframe effect. They are different things. The `playbackRate` property is used to control the speed of the animation playback, if you set it to 2, the animation will play twice as fast, if you set it to 0.5, the animation will play half as fast. The `duration` property of the keyframe effect is used to set the length of the animation in milliseconds, it is the total time it takes for the animation to complete one iteration.

