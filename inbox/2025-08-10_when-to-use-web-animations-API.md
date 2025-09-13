---
date: 2025-08-10
type: fact
aliases:
  -
hubs:
  - "[[web-animation]]"
---

# when to use web animations API

Below we have an element called `square` and we apply a simple animation called `moveAndRotate` to it.

Assuming this animation needs to be able to interact with the user, such as "play," "pause," "reverse play," and adjust the speed of the animation, we use event listeners and change the style to achieve these functions below.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="stylesheet" href="styles.css" />
    <title>Web Animations API</title>
    <script>
      document.addEventListener("DOMContentLoaded", () => {
        const square = document.querySelector(".square");
        const pauseButton = document.querySelector(".pauseButton");
        const playButton = document.querySelector(".playButton");
        const reverseButton = document.querySelector(".reverseButton");
        const durationInput = document.getElementById("durationInput");

        pauseButton.addEventListener("click", () => {
          square.classList.add("pause");
        });
        playButton.addEventListener("click", () => {
          square.classList.remove("pause");
        });
        reverseButton.addEventListener("click", () => {
          square.classList.toggle("reverse");
        });
        durationInput.addEventListener("input", (e) => {
          square.style.animationDuration = `${e.target.value}ms`;
        });
      });
    </script>
  </head>
  <body>
    <div class="square"></div>
    <button class="pauseButton">Pause</button>
    <button class="playButton">Play</button>
    <button class="reverseButton">Reverse</button>
    <input type="range" id="durationInput" step="100" min="500" max="5000" />
  </body>
</html>
```


```css
body {
  background-color: #1d1d1d;
  font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
    Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
  padding: 0;
  margin: 0;
}

* {
  box-sizing: border-box;
}

.square {
  width: 100px;
  height: 100px;
  background-color: gold;
  animation: 3s 1s infinite alternate both linear moveAndRotate;
}

.square.pause {
  animation-play-state: paused;
}
.square.reverse {
  animation-direction: alternate-reverse;
}

@keyframes moveAndRotate {
  0% {
    transform: translateX(0);
  }
  80% {
    background-color: blue;
  }
  100% {
    transform: translateX(calc(100vw - 100px)) rotate(360deg);
    background-color: crimson;
  }
}
```

You will see that this method is feasible, but there will be a choppy feeling when switching between the states of "pause," "play," and "reverse play" in the animation, as well as adjusting the speed. This is a native drawback of using CSS to control animations.

To achieve smoother animations and better control, we can use the Web Animations API. We will learn it in the other topic.

```
