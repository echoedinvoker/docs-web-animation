---
date: 2025-08-15
type: fact
aliases:
  -
hubs:
  - "[[web-animation]]"
---

# exercise: wheels

```js
document.addEventListener("DOMContentLoaded", () => {
  // ...

  async function addCar() {
    if (streetAnimation.playState !== "running" || document.querySelector(".car")) return;
    const car = document.createElement("div");
    car.classList.add("car");
    const carAnimation = car.animate([
      { transform: "translateX(-100vw)" },
      { transform: "translateX(100vw)" }
    ], {
      id: "car",
      duration: Math.random() * 4000 + 200,
      easing: "linear",
    })
    carWrapper.appendChild(car);
    await carAnimation.finished;
    car.remove();

    setTimeout(() => {
      if (streetAnimation.playState === "running") {
        addCar();
      }
    }, Math.random() * 3000 + 1000);
  }

  streetAnimation.ready.then(() => {
    if (streetAnimation.playState !== "running") return;
    addCar();
  })

  //...
});
```

In the example above, we implemented an animation of randomly adding vehicles and moving them on the street. Now we want to add a rotating animation to the wheels of the vehicles, the wheels are added to the vehicle using the `:before` and `:after` pseudo-elements.


```js
  async function addCar() {
    // ...

    const carAnimation = car.animate( ... )

    // add the wheels animation to the car pseudo-elements
    car.animate([
      { transform: "rotate(0)" },
      { transform: "rotate(360deg)" }
    ], {
      id: "car", // apply the same id as the car animation to prevent speeding up/down the animation when user speeds up/down the character (so different animations can use the same id ...)
      pseudoElement: ":before", // use pseudo-elements to only animate the wheels, not the whole car

      // the speed of the wheels should be based on the car animation duration, so we get the duration of the car animation from the animation object
      duration: carAnimation.effect.getComputedTiming().duration / 4,
      easing: "linear",
      iterations: Infinity // wheels should rotate infinitely
    });
    // ...
  }
```

Above, we only added animation to one of the wheels `:before`. We need to write the exact same animation for the other wheel. We can use a `forEach` loop to simplify the code, so we don't have to repeat writing the same animation code.


```js
    Array.from([":after", ":before"]).forEach((pseudo) => { // there are two wheels using pseudo-elements `:before` and `:after`, instead of creating two separate animations, we can use a forEach loop to iterate over both pseudo-elements and create the same animation for both wheels
      car.animate([
        { transform: "rotate(0)" },
        { transform: "rotate(360deg)" }
      ], {
        id: "car",
        pseudoElement: pseudo,
        duration: carAnimation.effect.getComputedTiming().duration / 4,
        easing: "linear",
        iterations: Infinity
      });
    });
```
