---
date: 2025-08-14
type: fact
aliases:
  -
hubs:
  - "[[web-animation]]"
---

# exercise: moving road and signs

```html
<!DOCTYPE html>
<html lang="en">
  ...
  <body>
    <div class="view">
      <div class="sky"> ... </div>

      <div class="background"> <!-- we want to move this -->
        <img src="assets/s1.png" alt="" />
      </div>

      <div class="ground">
        <div class="street"> <!-- we want to move this -->
          <div class="lines"></div>
        </div>
      </div>

      <div class="character"></div> <!-- already animated -->

      <div class="foreground"> <!-- we want to move this -->
        <img src="assets/s2.png" alt="" />
      </div>

    </div>
  </body>
</html>
```

```css
.ground {
  height: 50vh;
  position: relative;
}
.ground .street {
  background-image: linear-gradient(#464646, #242424);
  position: absolute;
  width: 200vw; /* double the width to allow for movement: translateX(0) -> translateX(-50%) */
  height: 100%;
}
.ground .street .lines {
  position: absolute;
  background-image: url("./assets/line.png");
  background-size: 12.5% auto;
  background-repeat: repeat-x;
  background-position: 0 0;
  height: 4%;
  width: 100%;
  top: 50%;
  opacity: 0.8;
}

.background {
  position: absolute;
  height: 50vh;
  width: 100%; /* translateX(100%) -> translateX(-100%) */
  top: 0;
}
.background img {
  position: absolute;
  height: 50%;
  bottom: -5px;
  left: 0;
}
.foreground {
  position: absolute;
  height: 50vh;
  width: 100%;  /* translateX(100%) -> translateX(-100%) */
  top: 50%;
}
.foreground img {
  position: absolute;
  height: 90%;
  bottom: 0px;
  left: 0;
}
```

As shown above, we have completed the animation of the character. Now we want to make the streets and road signs move together, and use the Web Animations API to achieve this.


```js
document.addEventListener("DOMContentLoaded", () => {
  const character = document.querySelector(".character");

  // get the street, background, and foreground elements
  const street = document.querySelector(".street");
  const background = document.querySelector(".background");
  const foreground = document.querySelector(".foreground");

  const characterAnimation = character.animate( ... );

  // create animations for the street, background, and foreground...
  const streetAnimation = street.animate([
    { transform: "translateX(0)" },
    { transform: "translateX(-50%)" }
  ], {
    easing: "linear",
    duration: 12000, // not like CSS, we don't need to use a variable because we can get the duration from the animation object easily
    iterations: Infinity,
  });
  const backgroundAnimation = background.animate([
    { transform: "translateX(100%)" },
    { transform: "translateX(-100%)" }
  ], {
    easing: "linear",
    duration: streetAnimation.effect.getComputedTiming().duration * 2,
    //        ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ get the duration of the street animation by using the effect's computed timing of the animation object
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

  async function jump() { ... }

  document.addEventListener("keyup", (event) => { ... });
});
