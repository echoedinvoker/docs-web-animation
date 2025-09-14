# Linear Timing Function

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="stylesheet" href="styles.css" />
    <title>Timing Functions</title>
    <script>
      document.addEventListener("DOMContentLoaded", () => {
        const progressCount = 10;
        const element = document.querySelector(".element");
        const progress = document.querySelector(".progress");
        const button = document.querySelector(".move-button");

        Array(progressCount + 1)
          .fill(null)
          .forEach((_null, i) => {
            const node = document.createElement("div");
            node.classList.add("progress-value");
            node.innerHTML = `<div class="number">${
              (i / progressCount) * 100
            }%<div>`;
            progress.append(node);
          });

        button.addEventListener("click", () => {
          element.classList.toggle("move");
        });
      });
    </script>
  </head>
  <body>
    <div class="progress"></div>
    <div class="track">
      <div class="element"></div>
    </div>
    <br />
    <button class="move-button">Move Element</button>
  </body>
</html>
```

```css
body {
  background-color: #1d1d1d;
  font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
    Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
  padding: 20px;
  --scale-color: lightgrey;
}

.track {
  position: relative;
  border: 2px solid var(--scale-color);
  height: 50px;
}

.element {
  width: 10px;
  height: 50px;
  background-color: greenyellow;
  transition: left 2s ;
  /*                  ^ we want to add `linear` timing function here */
  left: -5px;
  position: absolute;
}

.element.move {
  left: calc(100% - 5px);
}

.progress {
  position: relative;
  display: flex;
  justify-content: space-between;
}
.progress-value {
  color: var(--scale-color);
  position: relative;
  font-size: 14px;
  line-height: 16px;
  padding-top: 16px;
}
.progress-value .number {
  position: absolute;
  transform: translateX(-50%) translateY(-110%);
}
.progress-value::after {
  content: "";
  display: block;
  height: 10px;
  width: 2px;
  background-color: var(--scale-color);
}
```

Above example is a simple animation of an element moving from left to right on a track, we want to explore how to control the speed of this animation using a **linear timing function**.

- **linear**

The speed of the element is fixed, and the relationship between progress and time is linear (the rate of change of both is the same).

![linear-timing-func.png](../assets/imgs/linear-timing-func.png)

- **linear(0, 0.25, 1)**

We pass in three parameters, it will automatically divide the time evenly, so
  - the first parameter represents the progress at time 0%
  - the second parameter represents the progress at time 50%
  - the third parameter represents the progress at time 100%

![linear-3args.png](../assets/imgs/linear-3args.png)

- **linear(0, 0.25, 0.25, 1)**

Same, there are four parameters, so time will be divided into three equal parts 
  - the first parameter represents the progress at time 0%
  - the second parameter represents the progress at time 33.33%
  - the third parameter represents the progress at time 66.67%
  - the fourth parameter represents the progress at time 100%

You can see the second and third values are the same, so the progress will not change during the second third of the time, so the animation will pause for a while.

![linear-pause.png](../assets/imgs/linear-pause.png)

- **linear(0, 0.25 25%, 1)**

Similar to the first example `linear(0, 0.25, 1)`, but here we add `25%` to the second parameter, which means that the timing is 25% of the total time instead of 50%.

time     0% -> 25% -> 100%
progress 0% -> 25% -> 100%

![linear-specify-time.png](../assets/imgs/linear-specify-time.png)

- **linear(0, 0.25 25% 75%, 1)**

It means the progress is fixed at 25% from time 25% to time 75%, so the animation will pause for a while.

![duo-specified-time.png](../assets/imgs/duo-specified-time.png)

