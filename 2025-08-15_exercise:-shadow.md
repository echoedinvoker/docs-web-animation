# exercise: shadow

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

In the example above, we want to add a shadow element on the ground, so that when the character jumps, the shadow will also shrink accordingly.

## Create a shadow element on the ground

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
      ...
      <div class="ground">
        <div class="street">
          <div class="lines"></div>
        </div>
        <div class="shadow"></div> <!-- Add shadow element -->
      </div>
      ...
    </div>
  </body>
</html>
```

```css
.shadow {
  position: absolute;
  bottom: 0;
  top: 0;
  left: 0;
  right: 0;
  margin: auto;
  width: 20vh;
  aspect-ratio: 5;
  border-radius: 50%;
  background-color: black;
  opacity: 0.25;
  filter: blur(3px);
}
```

## Add the shadow animation for the jump

```js
document.addEventListener("DOMContentLoaded", () => {
  const character = document.querySelector(".character");
  const street = document.querySelector(".street");
  const background = document.querySelector(".background");
  const foreground = document.querySelector(".foreground");

  const characterAnimation = character.animate( ... );

  const streetAnimation = street.animate( ... );
  const backgroundAnimation = background.animate( ... );
  const foregroundAnimation = foreground.animate( ... );

  async function jump() {
    if (
      streetAnimation.playState !== "running" ||
      character.getAnimations().find(anim => anim.id === "jump")
    ) return;
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

    // adding shadow animation into jump function
    // the shadow animation should match the jump animation
    // instead of writing the same options again, better to use the computed timing of the jump animation
    const { duration, easing, iterations, direction } = jumpAnimation.effect.getComputedTiming();
    document.querySelector('.shadow').animate([
      {
        transform: "scale(1)",
      },
      {
        transform: "scale(0.5)",
      },
    ], { duration, easing, iterations, direction }); // using the options from the jump animation

    await jumpAnimation.finished;
    characterAnimation.play();
    character.classList.remove("jump");
  }

  //...

  document.addEventListener("keyup", (event) => {
    switch (event.code) {
      case "ArrowUp":
        jump();
        break;
      //...
    }
  });
});
