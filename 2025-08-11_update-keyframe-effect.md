---
date: 2025-08-11
type: fact
aliases:
  -
hubs:
  - "[web-animation](./web-animation.md)"
---

# update keyframe effect

In [buttons to call animation object methods](./2025-08-11_buttons-to-call-animation-object-methods.md), we can use the *animation object* methods to control the animation. But what if we want to change the animation itself?

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
      duration: 3000,
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
  console.log(squareAnimation); // check animation effect on the console
  squareAnimation.pause();

  const buttons = document.querySelectorAll(".button");
  buttons.forEach((button) => {
    button.addEventListener("click", () => {
      if (button.classList.contains("play")) {
        squareAnimation.play();
      }
      if (button.classList.contains("pause")) {
        squareAnimation.pause();
      }
      if (button.classList.contains("cancel")) {
        squareAnimation.cancel();
      }
      if (button.classList.contains("reverse")) {
        squareAnimation.reverse();
      }
      if (button.classList.contains("finish")) {
        squareAnimation.finish();
      }
    });
  });

  const playbackRateInput = document.getElementById("playbackRateInput");
  const playbackRateInputValue = document.getElementById(
    "playbackRateInputValue"
  );

  playbackRateInput.addEventListener("input", (e) => {
    squareAnimation.updatePlaybackRate(e.target.value);
    playbackRateInputValue.value = e.target.value;
  });
});
```

```
Animation {
    ...
    effect: KeyframeEffect {     // you can see some useful methods under `Animation.effect`
        ...                      // we can use them to change the animation effect (keyframes effect in this case)
        [Prototype](./Prototype.md): KeyframeEffect {
            ...
            getKeyframes: ƒ getKeyframes()
            setKeyframes: ƒ setKeyframes()
            [Prototype](./Prototype.md): AnimationEffect {
                ...
                getComputedTiming: ƒ getComputedTiming()
                getTiming: ƒ getTiming()
                updateTiming: ƒ updateTiming()
            }
        }
    }
}
```

```js
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

      <!-- add KeyframeEffect Section -->
      <div class="section">
        <h3>Animation Effect (KeyframeEffect, DVD)</h3>
        <label for="durationInput">Duration</label>
        <input type="range" id="durationInput" step="500" min="0" max="10000" />
        <output id="durationInputValue" for="durationInput">0</output>
        <br /><br />
        <label>Infinite</label>
        <input type="checkbox" id="infiniteInput" />
        <br /><br />
        <button class="button changeAnimation">Change Animation</button>
      </div>
    </div>
  </body>
</html>
```

AnimationEffect contains the keyframes and timing properties of the animation. We can change the keyframes using `setKeyframes()` and change the timing properties using `updateTiming()` as shown below.

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
      duration: 3000,
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

  const buttons = document.querySelectorAll(".button");
  buttons.forEach((button) => {
    button.addEventListener("click", () => {
      if (button.classList.contains("play")) {
        squareAnimation.play();
      }
      if (button.classList.contains("pause")) {
        squareAnimation.pause();
      }
      if (button.classList.contains("cancel")) {
        squareAnimation.cancel();
      }
      if (button.classList.contains("reverse")) {
        squareAnimation.reverse();
      }
      if (button.classList.contains("finish")) {
        squareAnimation.finish();
      }

      // Change the KeyframeEffect content (override the existing keyframes)
      if (button.classList.contains("changeAnimation")) {
        squareAnimation.effect.setKeyframes([
          {
            transform: "translateY(0)",
        //                       ^ the concept is to replace X axis with Y axis, so the square moves vertically
          },
          {
            backgroundColor: "greenyellow",
            offset: 0.8,
          },
          {
            transform: "translateY(calc(100vh - 100px)) rotate(360deg)",
            backgroundColor: "purple",
          },
        ]);
      }
    });
  });

  //...

  // Change the options of the KeyframeEffect
  const durationInput = document.getElementById("durationInput");
  const durationInputValue = document.getElementById("durationInputValue");
  durationInput.addEventListener("input", (e) => {
    squareAnimation.effect.updateTiming({
      duration: +e.target.value,
    });
    durationInputValue.value = e.target.value;
  });
  const infiniteInput = document.getElementById("infiniteInput");
  infiniteInput.addEventListener("change", (e) => {
    squareAnimation.effect.updateTiming({
      iterations: e.target.checked ? Infinity : 2,
    });
  });
});
```
