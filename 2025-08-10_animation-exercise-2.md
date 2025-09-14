---
date: 2025-08-10
type: fact
aliases:
  -
hubs:
  - "[web-animation](./web-animation.md)"
---

# animation exercise 2

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
}
```

Based on the foundation of [animation exercise 1](./2025-08-10_animation-exercise-1.md) as above, we want to add an animation to this character to make it look like it is running.

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
  animation: run 1s infinite; /* apply the animation `run` infinite times */
}

/* create a keyframes animation to switch the frame of the character sprite */
@keyframes run {
  0% {
    background-position: 0 0; /* the first frame */
  }
  100% {
    background-position: calc(var(--char-width) * -7) 0px;
    /*                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^ the last frame */
  }
}
```

But this animation will not look good, because it will transition between frames smoothly, which is not what we want for a sprite animation. We want to switch frames in steps, so that each frame is shown for a fixed duration.

And we need to decide which mode we want to use for the steps.

![choose-timing-func-step.png](../assets/imgs/choose-timing-func-step.png)
Because `jump-none` leaves some duration for each frame, it is the best choice for sprite animations.


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
  /*                         ^^^^^^^^^^^^^^^^^^^ 
     This is the key part that makes the animation work in steps */
}

@keyframes run {
  0% { /* because of `jump-none` steps, this frame will be hold about 1/8 of the time */
    background-position: 0 0;
  }

  /* every frame will be hold for 1/8 of the time and no transition will happen */

  100% { /* because of `jump-none` steps, this frame will be hold about 1/8 of the time */
    background-position: calc(var(--char-width) * -7) 0px;
  }
}
```



