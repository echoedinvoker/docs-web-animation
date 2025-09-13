---
date: 2025-08-15
type: fact
aliases:
  -
hubs:
  - "[[web-animation]]"
---

# randomizing our car speed and occurence

In the [[2025-08-15_exercise:-car|exercise: car]], we created a car element and its animation, but the speed is fixed. We can randomize its speed and appearance time.

## Randomizing speed

```js
document.addEventListener("DOMContentLoaded", () => {
  //...

  async function addCar() {
    const car = document.createElement("div");
    car.classList.add("car");
    const carAnimation = car.animate([
      { transform: "translateX(-100vw)" },
      { transform: "translateX(100vw)" }
    ], {
      duration: Math.random() * 4000 + 200, // Random duration between 200ms and 4200ms
      easing: "linear",
    })
    carWrapper.appendChild(car);
    await carAnimation.finished;
    car.remove();
  }

  addCar();

  //...
});
```

## Preventing adding cars when the scene is paused

In some scenarios where the animation is not played from the beginning, we need to ensure that vehicles are only added when the animation is playing, otherwise it will result in vehicles being added to the screen when the animation is paused. (However, in our case, the animation is played from the beginning, so this step can be omitted)

```js
document.addEventListener("DOMContentLoaded", () => {
  //...
  const street = document.querySelector(".street");

  //...

  const streetAnimation = street.animate( ... );

  //...

  async function addCar() { ... }

  // Only add a car if the street animation is running
  streetAnimation.ready.then(() => {
    if (streetAnimation.playState !== "running") return;
    addCar();
  })

  //...
});
```

## Repeating the car addition

Because we only execute `addCar()` once above, there will be no more vehicles appearing afterwards. In order to continuously have vehicles appear, we can use `addCar()` within `addCar()` to repeatedly add vehicles, and use `setTimeout()` to control the interval at which vehicles appear.

```js
document.addEventListener("DOMContentLoaded", () => {
  //...

  async function addCar() {
    //...
    car.remove();

    setTimeout(() => {
      if (streetAnimation.playState === "running") {
        addCar();
      }
    }, Math.random() * 3000 + 1000); // After car removal, wait between 1000ms and 4000ms before adding a new car
  }

  //...
});
```

## Adding car when the animation is resumed

Because `addCar()` will be skipped when the animation is paused due to streetAnimation.playState !== "running", so when we resume the animation, we should make sure to call `addCar()` again to add new cars.


```js
document.addEventListener("DOMContentLoaded", () => {

  // ...

  function togglePause() {
    //...

    addCar();  // Ensure a new car is added when the animation is resumed
  }

  // ...

  async function addCar() {
    // only add a car if the street animation is running (togglePause might pause it)
    if (streetAnimation.playState !== "running" || document.querySelector(".car")) return;

    //...
  }

  //...

});
```

## Preventing speeding up/down the car with the character animation

We previously implemented the functionality of accelerating and decelerating the running speed of characters, along with the speed of the entire road and background animations. However, this would cause the animation speed of vehicles to also accelerate or decelerate, which is not the behavior we want. We can exclude the animation of vehicles in the acceleration and deceleration functions, allowing them to maintain a fixed speed.


```js
document.addEventListener("DOMContentLoaded", () => {

  // ...

  function speedUp() {
    document.getAnimations().forEach((anim) => {
      if (
        streetAnimation.playbackRate >= 3 ||
        anim.id === "car" // Prevent speeding up the car animation
      ) return;
      anim.playbackRate += 0.1;
    });
  }

  function speedDown() {
    document.getAnimations().forEach((anim) => {
      if (
        streetAnimation.playbackRate <= 0.5 ||
        anim.id === "car" // Prevent slowing down the car animation
      ) return;
      anim.playbackRate -= 0.1;
    });
  }

  // ...

  async function addCar() {
    // ...
    const carAnimation = car.animate([
      { transform: "translateX(-100vw)" },
      { transform: "translateX(100vw)" }
    ], {
      id: "car", // Assign an ID to the car animation for identification in speed control handlers
      duration: Math.random() * 4000 + 200,
      easing: "linear",
    })
    //...
  }

  //...
});
