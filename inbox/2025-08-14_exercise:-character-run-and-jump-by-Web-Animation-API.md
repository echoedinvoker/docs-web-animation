---
date: 2025-08-14
type: fact
aliases:
  -
hubs:
  - "[[web-animation]]"
---

# exercise: character run and jump by Web Animation API

Before, we used CSS to implement the running and jumping animations of the character. This time, we will use the Web Animations API to achieve the same functionality. This exercise will help you become familiar with the basic usage of the Web Animations API.

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
      <div class="character"></div> <!-- Character element -->
      <div class="foreground">
        <img src="assets/s2.png" alt="" />
      </div>
    </div>
  </body>
</html>
```

```css
.character {
  --char-width: 25vh;
  position: absolute;
  bottom: 0;
  top: 0;
  left: 0;
  right: 0;
  margin: auto;
  background-image: url("./assets/7888220.png"); /* Character sprite sheet */
  background-size: calc(var(--char-width) * 8);
  background-position: calc(var(--char-width) * -5) 0px;
  background-repeat: no-repeat;
  width: var(--char-width);
  aspect-ratio: 0.535;
}
```


## Running Animation

Instead of using CSS animations, we will use the Web Animations API to animate the character's running and jumping actions:

```js
  const character = document.querySelector(".character");

  // Create a running animation for the character
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
      easing: "steps(8, jump-none)", // use steps for sprite animation
    }
  );
});
```

## Jumping Animation

Jumping animation is triggered by pressing the "ArrowUp" key. So we need to listen for the keyup event and create a jump animation using the Web Animations API.

```js
document.addEventListener("DOMContentLoaded", () => {
  const character = document.querySelector(".character");
  const characterAnimation = character.animate( ... );

  // Function to create a jump animation, it will be triggered by the "ArrowUp" key
  function jump() {
    const jumpAnimation = character.animate([
      {
        transform: "translateY(0)",
      },
      {
        transform: "translateY(-70px)",
      },
    ],
    {
      duration: 500,
      easing: "ease-in-out",
      iterations: 2,
      direction: "alternate",
    });
  }
  // listen for keyup event to trigger the jump
  document.addEventListener("keyup", (event) => {
    switch (event.code) {
      case "ArrowUp":
        jump(); // Call the jump function when the "ArrowUp" key is pressed
        break;
      default:
        break;
    }
  });
});
```

### prevent multiple jumps

But now we can jump during the jumping animation, which is not what we want. We need to prevent multiple jumps by checking if the jump animation is already running.

```js
document.addEventListener("DOMContentLoaded", () => {
  const character = document.querySelector(".character");
  const characterAnimation = character.animate( ... );

  function jump() {
    if (character.getAnimations().find(anim => anim.id === "jump")) return; // Prevent multiple jumps by checking for existing jump animation with ID "jump"
    const jumpAnimation = character.animate([ ... ],
    {
      id: "jump", // Assign an ID to the animation of the jump
      duration: 500,
      easing: "ease-in-out",
      iterations: 2,
      direction: "alternate",
    });
  }

  document.addEventListener("keyup", (event) => { ... });
});
```

### prevent running animation during jump

Not like the CSS animation, the jump animation will not overlap with the running animation. So we need to pause the running animation when the jump animation is triggered and resume it after the jump animation is finished.

```js
document.addEventListener("DOMContentLoaded", () => {
  const character = document.querySelector(".character");
  const characterAnimation = character.animate( ... );

  async function jump() {
  //^^^
    if (character.getAnimations().find(anim => anim.id === "jump")) return;
    characterAnimation.pause(); // Pause the running animation before jumping
    const jumpAnimation = character.animate([ ... ], { ... });
    await jumpAnimation.finished; // Wait for the jump animation to finish (you can also use event `finish`)
    characterAnimation.play(); // Resume the running animation after the jump
  }

  document.addEventListener("keyup", (event) => { ... });
});
```

### Fixed frame during jump

Every time we jump, we will determine the background position of the jump based on the current running animation background position. This will result in a different background position for each jump, which is not the effect we want. Therefore, we need to fix the background position when jumping.


```css
/* fixed character jump background position to show the specific frame for jumping */
.character.jump {
  background-position: calc(var(--char-width) * -4) 0px !important;
}
```


```js
document.addEventListener("DOMContentLoaded", () => {
  const character = document.querySelector(".character");
  const characterAnimation = character.animate( ... );

  async function jump() {
    if (character.getAnimations().find(anim => anim.id === "jump")) return;
    characterAnimation.pause();
    character.classList.add("jump"); // Add a class to change the background position for the jump animation
    const jumpAnimation = character.animate( ... );
    await jumpAnimation.finished;
    characterAnimation.play();
    character.classList.remove("jump"); // Remove the class after the jump animation is done
  }

  document.addEventListener("keyup", (event) => { ... });
});
```



