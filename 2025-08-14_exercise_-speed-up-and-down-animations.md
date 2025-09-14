# exercise: speed up and down animations

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

We want to make the left and right arrow keys accelerate or decelerate the running speed (equivalent to the playbackRate of all animations).

```js
document.addEventListener("DOMContentLoaded", () => {
  const character = document.querySelector(".character");
  const street = document.querySelector(".street");
  const background = document.querySelector(".background");
  const foreground = document.querySelector(".foreground");

  const characterAnimation = character.animate(...);

  const streetAnimation = street.animate(...);

  async function jump() {...}

  function togglePause() {...}

  // Speed up and down functions by adjusting playbackRate of animation object
  function speedUp() {
    if (streetAnimation.playbackRate >= 3) return; // Limit max speed
    document.getAnimations().forEach((anim) => {
      anim.playbackRate += 0.1;
    });
  }
  function speedDown() {
    if (streetAnimation.playbackRate <= 0.5) return; // Limit min speed
    document.getAnimations().forEach((anim) => {
      anim.playbackRate -= 0.1;
    });
  }

  document.addEventListener("keyup", (event) => {
    switch (event.code) {
      case "ArrowUp":
        jump();
        break;
      case "Space":
        togglePause();
        break;

      // Speed up or down based on arrow keys
      case "ArrowLeft":
        speedDown();
        break;
      case "ArrowRight":
        speedUp();
        break;

      default:
        break;
    }
  });
});
```


## Assuming character will feel tired after a while, and speed will decrease automatically

We can set an interval to cal `speedDown` every interval, and the rate won't below 0.5 by the definition of `speedDown`.


```js
document.addEventListener("DOMContentLoaded", () => {
  //...

  function speedUp() { ... }

  function speedDown() { ... }

  // Automatically decrease speed every second
  setInterval(() => {
    if (streetAnimation.playState !== "running") return;
    speedDown();
  }, 1000);

  document.addEventListener("keyup", (event) => { ... });
});
```
