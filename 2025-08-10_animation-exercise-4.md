---
date: 2025-08-10
type: fact
aliases:
  -
hubs:
  - "[web-animation](./web-animation.md)"
---

# animation exercise 4

```css
.ground {
  height: 50vh;
  position: relative;
}
.ground .street {
  background-image: linear-gradient(#464646, #242424);
  position: absolute;
  width: 200vw; /* double the width of the viewport */
  height: 100%;
  animation: moveStreet 10s linear infinite;
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

/* background and foreground are not inside the ground, so we need to animate them separately */

.background { /* just a container for the background image, no any styles for this */
  position: absolute;
  height: 50vh;
  width: 100%; /* same width as viewport */
  top: 0;
}
.background img {
  position: absolute;
  height: 50%;
  bottom: -5px;
  left: 0;
}
.foreground { /* just a container for the foreground image, no any styles for this */
  position: absolute;
  height: 50vh;
  width: 100%; /* same width as viewport */
  top: 50%;
}
.foreground img {
  position: absolute;
  height: 90%;
  bottom: 0px;
  left: 0;
}
```

![background-pos-100.png](../assets/imgs/background-pos-100.png)

![background-neg-100.png](../assets/imgs/background-neg-100.png)

```css
:root {
  --duration: 10s; /* set the duration var for street, background, and foreground animations */
}

.ground {
  height: 50vh;
  position: relative;
}
.ground .street {
  background-image: linear-gradient(#464646, #242424);
  position: absolute;
  width: 200vw;
  height: 100%;
  animation: moveStreet var(--duration) linear infinite;
  /*                    ^^^^^^^^^^^^^^^ use variable for duration */
}
.ground .street .lines { ... }

.background {
  position: absolute;
  height: 50vh;
  width: 100%;
  top: 0;
  animation: moveBackground var(--duration) linear infinite; /* apply `moveBackground` animation */
}
.background img { ... }

.foreground {
  position: absolute;
  height: 50vh;
  width: 100%;
  top: 50%;
  animation: moveForeground var(--duration) linear infinite; /* apply `moveForeground` animation */
}

.foreground img { ...}

@keyframes moveStreet {
  0% {
    transform: translateX(0):
  }
  100% {
    transform: translateX(-50%);
  }
}

/* create the keyframes for background animations */
@keyframes moveBackground {
  0% {
    transform: translateX(100%);
  }
  100% {
    transform: translateX(-100%);
  }
}

/* create the keyframes for foreground animation */
@keyframes moveForeground {
  0% {
    transform: translateX(100%);
  }
  100% {
    transform: translateX(-100%);
  }
}
```

Above, we implemented animations for the background and foreground, but it is noticeable that their speed is faster than the animation for the street. This is because, in the same duration, the background and foreground moved a distance of 200, while the street only moved a distance of 50. Additionally, the width of the street is 200vw, while the background and foreground have a width of 100vw. Therefore, we need to adjust their duration to make their speeds consistent.


```css
.ground { ... }
.ground .street {
  background-image: linear-gradient(#464646, #242424);
  position: absolute;
  width: 200vw;
  height: 100%;
  animation: moveStreet var(--duration) linear infinite;
  /*                    ^^^^^^^^^^^^^^^ keep the same duration for street */
}
.ground .street .lines { ... }

.background {
  position: absolute;
  height: 50vh;
  width: 100%;
  top: 0;
  animation: moveBackground calc(var(--duration) * 2) linear infinite;
  /*                        ^^^^^^^^^^^^^^^^^^^^^^^^^ (100 * 2) / (200 * 0.5) = 2 */
}
.background img { ... }

.foreground {
  position: absolute;
  height: 50vh;
  width: 100%;
  top: 50%;
  animation: moveForeground calc(var(--duration) * 2 * 0.4) linear infinite;
  /*                                                 ^^^^^ because foreground closed to the user, it should be faster than the street a bit, so we add one more multiplier to adjust the speed */
}
.foreground img { ... }

```
