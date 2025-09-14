# exercise: pause all scene

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="stylesheet" href="styles.css" />
    <title>Web Animations API Exercise</title>
    <script src="script.js"></script>
  </head>
  <body>
    <div class="view">
      <div class="sky">
        <img src="assets/cloud1.png" class="cloud1" alt="" />
        <img src="assets/cloud2.png" class="cloud2" alt="" />
      </div>
      <div class="background">
        <img src="assets/s1.png" alt="" />
      </div>
      <div class="ground">
        <div class="street">
          <div class="lines"></div>
        </div>
      </div>
      <div class="character"></div>
      <div class="foreground">
        <img src="assets/s2.png" alt="" />
      </div>
    </div>
  </body>
</html>
```

```js
document.addEventListener("DOMContentLoaded", () => {
  const character = document.querySelector(".character");
  const street = document.querySelector(".street");
  const background = document.querySelector(".background");
  const foreground = document.querySelector(".foreground");

  const characterAnimation = character.animate(
    [
      {
        backgroundPosition: "0 0",
      },
      {
        backgroundPosition: "calc(var(--char-width) * -7) 0",
      },
    ],
    {
      duration: 1000,
      iterations: Infinity,
      easing: "steps(8, jump-none)",
    }
  );

  const streetAnimation = street.animate([
    { transform: "translateX(0)" },
    { transform: "translateX(-50%)" }
  ], {
    easing: "linear",
    duration: 12000,
    iterations: Infinity,
  });
  const backgroundAnimation = background.animate([
    { transform: "translateX(100%)" },
    { transform: "translateX(-100%)" }
  ], {
    easing: "linear",
    duration: streetAnimation.effect.getComputedTiming().duration * 2,
    iterations: Infinity,
  });
  const foregroundAnimation = foreground.animate([
    { transform: "translateX(100%)" },
    { transform: "translateX(-100%)" }
  ], {
    easing: "linear",
    duration: streetAnimation.effect.getComputedTiming().duration * 1.5,
    iterations: Infinity,
  });

  async function jump() {
    if (character.getAnimations().find(anim => anim.id === "jump")) return;
    characterAnimation.pause();
    character.classList.add("jump");
    const jumpAnimation = character.animate([
      {
        transform: "translateY(0)",
      },
      {
        transform: "translateY(-70px)",
      },
    ],
    {
      id: "jump",
      duration: 500,
      easing: "ease-in-out",
      iterations: 2,
      direction: "alternate",
    });
    await jumpAnimation.finished;
    characterAnimation.play();
    character.classList.remove("jump");
  }

  document.addEventListener("keyup", (event) => {
    switch (event.code) {
      case "ArrowUp":
        jump();
        break;
      default:
        break;
    }
  });
});
```

In the example above, there are already many animations. We want to implement a function where when the space bar is pressed, all animations can be paused, and pressing the space bar again will resume the animations.

```js
document.addEventListener("DOMContentLoaded", () => {

  //...

  // create a function to toggle pause on all animations
  function togglePause() {
    document.getAnimations().forEach((anim) => {
      if (anim.playState === "running") {
        anim.pause();
      } else {
        anim.play();
      }
    })
  }

  document.addEventListener("keyup", (event) => {
    switch (event.code) {
      case "ArrowUp":
        jump();
        break;
      // toggle pause on all animations when space is pressed
      case "Space":
        togglePause();
        break;
      default:
        break;
    }
  });
});
```

However, even in a paused state, we can still jump, and the character will perform a running animation after jumping. We need to avoid this situation from happening.


```js
document.addEventListener("DOMContentLoaded", () => {

  // ...

  async function jump() {
    if (
      streetAnimation.playState !== "running" || // check if street animation is running, if not, skip jump
      character.getAnimations().find(anim => anim.id === "jump")
    ) return;
    characterAnimation.pause();
    character.classList.add("jump");
    const jumpAnimation = character.animate( ... );
    await jumpAnimation.finished;
    characterAnimation.play();
    character.classList.remove("jump");
  }

  function togglePause() { ... }

  document.addEventListener("keyup", (event) => {
    switch (event.code) {
      case "ArrowUp":
        jump();
        break;
      case "Space":
        togglePause();
        break;
      default:
        break;
    }
  });
});
```
