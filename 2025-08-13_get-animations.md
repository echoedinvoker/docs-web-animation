# get animations

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
    <div class="square">
      <div class="square-2"></div>
    </div>
    <div class="square-3"></div>
  </body>
</html>
```

```js
document.addEventListener("DOMContentLoaded", async () => {
  const element = document.querySelector(".square");
  const element2 = document.querySelector(".square-3");

  element2.animate(
    [
      {
        backgroundColor: "red",
      },
      {
        backgroundColor: "yellow",
      },
    ],
    {
      duration: 2000,
      direction: "alternate",
      iterations: Infinity,
    }
  );

  element.animate(
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
      iterations: 2,
      easing: "linear",
      composite: "add",
      iterationComposite: "accumulate",
      timeline: document.timeline,
    }
  );
});
```

```css
.square {
  width: 100px;
  height: 100px;
  background-color: gold;
}

.square-2 {
  width: 30px;
  height: 30px;
  background-color: purple;
  animation: 2s bounce infinite alternate ease-in-out;
}
.square-3 {
  width: 70px;
  height: 70px;
  animation: 2s bounce infinite alternate ease-in-out;
}

@keyframes bounce {
  0% {
    transform: translateY(0);
  }
  100% {
    transform: translateY(70px);
  }
}
```

![multi-animations.png](../assets/imgs/multi-animations.png)

In the example above, we can see that there are three square elements, and a combination of CSS and Web Animations API is used to achieve multiple animation effects. We can use the `getAnimations()` method of the document to obtain all animations in the current document.

```js
document.addEventListener("DOMContentLoaded", async () => {
  // ...

  console.log(document.getAnimations());
  console.log(element.getAnimations());
  //          ^^^^^^^ you can also call `getAnimations()` on an specific element, it'll return the animations that are applied to that element.
});
```

```
[CSSAnimation, CSSAnimation, Animation, Animation]
[Animation]
```

The element above only returns animations that apply to itself, but the element has child elements that also have animations. If you want to get animations for all child elements, you can pass in the `{ subtree: true }` option.

```js
document.addEventListener("DOMContentLoaded", async () => {
  // ...

  console.log(document.getAnimations());
  console.log(element.getAnimations({ subtree: true }));
  //                                ^^^^^^^^^^^^^^^^^
});
```

```
[CSSAnimation, CSSAnimation, Animation, Animation]
[CSSAnimation, Animation]
 ^^^^^^^^^^^^ CSS animation applied to the child element of the element
```

After obtaining the animations, we can do some operations on them, such as pausing, resuming, or changing the playback rate. For example, we can add buttons to control the speed of all animations in the document.


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
    <div class="square">
      <div class="square-2"></div>
    </div>
    <div class="square-3"></div>

    <!-- add controls to change the speed of all animations -->
    <div class="control">
      <button class="button decrease">-</button>
      <button class="button increase">+</button>
    </div>
  </body>
</html>
```

```js
document.addEventListener("DOMContentLoaded", async () => {
  const element = document.querySelector(".square");
  const element2 = document.querySelector(".square-3");

  element2.animate( ... );

  element.animate( ... );

  // let buttons control the speed of all animations by animation.updatePlaybackRate() to change the playback rate of all animations in the document.
  const buttons = document.querySelectorAll(".button")
  buttons.forEach((button) => {
    button.addEventListener("click", () => {
      if (button.classList.contains("increase")) {
        document.getAnimations().forEach((animation) => {
          animation.updatePlaybackRate(animation.playbackRate + 0.2);
        })
      }
      if (button.classList.contains("decrease")) {
        document.getAnimations().forEach((animation) => {
          animation.updatePlaybackRate(animation.playbackRate - 0.2);
        })
      }
    })
  });
});
```

> Even the CSSAnimation object can use the `updatePlaybackRate()` method to update the animation playback rate, just like the Animation object in the Web Animations API.


