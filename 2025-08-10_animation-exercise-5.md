---
date: 2025-08-10
type: fact
aliases:
  -
hubs:
  - "[web-animation](./web-animation.md)"
---

# animation exercise 5

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="stylesheet" href="styles.css" />
    <title>Animations Exercise</title>
    <script>
      document.addEventListener("DOMContentLoaded", () => {});
    </script>
  </head>
  <body>
    <div class="view">

      <!-- ... -->

      <div class="character"></div>

      <!-- ... -->

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
  background-image: url("./assets/7888220.png");
  background-size: calc(var(--char-width) * 8);
  background-position: calc(var(--char-width) * -4) 0px;
  background-repeat: no-repeat;
  width: var(--char-width);
  aspect-ratio: 0.535;
  animation: run 1s infinite steps(8, jump-none);
}

@keyframes run {
  0% {
    background-position: 0 0;
  }
  100% {
    background-position: calc(var(--char-width) * -7) 0px;
  }
}
```

## Implement a jump

First, we need to add a listener for the space key to trigger a jump. We can do this by adding an event listener for the `keyup` event on the document.

```js
      document.addEventListener("DOMContentLoaded", () => {
        document.addEventListener("keydown", (event) => {
          if (event.code === "Space") {
            console.log("Character jumped!"); // here we do a test to see if the event is triggered
          }
        });
      });
```

After testing, we can implement the jump by adding a class to the character that will trigger a CSS animation.

```js
      document.addEventListener("DOMContentLoaded", () => {
        const character = document.querySelector(".character"); // get the character element
        document.addEventListener("keydown", (event) => {
          if (event.code === "Space") {
            character.classList.add("jump"); // add the class `jump` to the character when the space key is pressed
          }
        });
      });
```


```css
.character { ... }


/* apply the jump animation when character has the class `jump` */
.character.jump {
  animation: jump 0.5s ease-in-out 2 alternate;
}

/* create a jump animation */
@keyframes jump {
  0% {
    transform: translateY(0);
  }
  100% {
    transform: translateY(-10vh);
  }
}
```

## Recover the `run` animation after jumping

But now we have a problem: after the jump, the character will stay in the jumping frame and not return to the running animation. That's because the `jump` animation overrides the `run` animation. To fix this, we can listen for the end of the jump animation and remove the `jump` class so that the character can return to the running animation.

```js
      document.addEventListener("DOMContentLoaded", () => {
        const character = document.querySelector(".character");
        document.addEventListener("keydown", (event) => {
          if (event.code === "Space") {
            character.classList.add("jump");
          }
        });

        // Listen for the end of the jump animation to remove the class `jump`
        // so the character can apply the `run` animation again
        document.addEventListener("animationend", (event) => {
          if (event.animationName === "jump") {
            character.classList.remove("jump");
          }
        });
      });
```

## Prevent multiple jumps

To prevent multiple jumps, you can modify the JavaScript code to check if the character is already jumping before adding the jump class:

```js
      document.addEventListener("DOMContentLoaded", () => {
        const character = document.querySelector(".character");
        document.addEventListener("keydown", (event) => {
          if (event.code === "Space" && !character.classList.contains("jump")) {
          //                         ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ to prevent multiple jumps
            character.classList.add("jump");
          }
        });
        document.addEventListener("animationend", (event) => {
          if (event.animationName === "jump") {
            character.classList.remove("jump");
          }
        });
      });
```

## Specify frame for jumping

```css
.character {
  --char-width: 25vh;
  position: absolute;
  bottom: 0;
  top: 0;
  left: 0;
  right: 0;
  margin: auto;
  background-image: url("./assets/7888220.png");
  background-size: calc(var(--char-width) * 8);
  background-position: calc(var(--char-width) * -4) 0px;
  /*                                            ^^ if you want to change the frame when jumping */
  /*                                               you can change this value to get a different jumping frame */
  background-repeat: no-repeat;
  width: var(--char-width);
  aspect-ratio: 0.535;
  animation: run 1s infinite steps(8, jump-none);
}
```
