---
date: 2025-08-15
type: fact
aliases:
  -
hubs:
  - "[web-animation](./web-animation.md)"
---

# exercise: car

## Create a static car and its wheels

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
        <div class="shadow"></div>
      </div>
      <div class="character"></div>

      <div class="car-wrapper"> <!-- the location is important, it should be on top of the ground and character, but below the foreground -->
        <div class="car"></div>
      </div>

      <div class="foreground">
        <img src="assets/s2.png" alt="" />
      </div>
    </div>
  </body>
</html>
```

```css
.car-wrapper {
  position: relative;
}
.car {
  position: absolute;
  bottom: 3%;
  left: 0;
  right: 0;
  margin: auto;
  background-image: url("./assets/car.png");
  --car-width: 120vh; /* width is based on the viewport height */
  background-size: contain;
  width: var(--car-width);
  aspect-ratio: 2.3; /* ratio is fixed for the specific car image */
}
.car:after {
  content: "";
  background-image: url("./assets/wheel.png");
  background-size: contain;
  width: 15.2%; /* width is based on the car width because the wheel is the psueduo-element of the car */
  aspect-ratio: 1;
  bottom: 4%; /* use % to position the wheel relative to the car */
  left: 19%; /* use % to position the wheel relative to the car, this is the left wheel */
  position: absolute;
}
.car:before {
  content: "";
  background-image: url("./assets/wheel.png");
  background-size: contain;
  width: 15.2%;
  aspect-ratio: 1;
  bottom: 4%;
  right: 9.9%; /* use % to position the wheel relative to the car, this is the right wheel */
  position: absolute;
}
```

## Dynamically create a car and its animation

The above is to adjust the position and size of the car and wheel in a static state. Next, we will dynamically create the car element and make it move.

```html
  <body>
    <div class="view">
      ...
      <div class="car-wrapper">
        <!-- <div class="car"></div> --> <!-- This will be dynamically created by JavaScript -->
      </div>
      ...
    </div>
  </body>
  ```

  ```js
document.addEventListener("DOMContentLoaded", () => {
  ...
  const carWrapper = document.querySelector(".car-wrapper"); // get the car wrapper element
  ...
  // create a function to create a car element and its animation, then add it into the carWrapper
  function addCar() {
    const car = document.createElement("div");
    car.classList.add("car");
    car.animate([
      { transform: "translateX(-100vw)" },
      { transform: "translateX(100vw)" }
    ], {
      duration: 2000,
      easing: "linear",
    })
    carWrapper.appendChild(car);
  }

  addCar();

  ...
});
```

## Remove the car after the animation completes

After the animation ends, the car element will return to the center of the screen and stop moving, which is not the effect we want. We hope the car element will be removed after the animation ends.

```js
...
  async function addCar() {
//^^^^^
    const car = document.createElement("div");
    car.classList.add("car");
    const carAnimation = car.animate([
      { transform: "translateX(-100vw)" },
      { transform: "translateX(100vw)" }
    ], {
      duration: 2000,
      easing: "linear",
    })
    carWrapper.appendChild(car);

    await carAnimation.finished; // wait for the animation to finish
    car.remove(); // remove the car element after the animation completes
  }

  addCar();

...

```

