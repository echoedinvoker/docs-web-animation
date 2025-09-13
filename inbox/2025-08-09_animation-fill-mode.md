---
date: 2025-08-09
type: fact
aliases:
  -
hubs:
  - "[[web-animation]]"
---

# animation fill mode

## Fill mode: Forwards

```css
.square {
  width: 100px;
  height: 100px;
  background-color: gold;
  animation: move 2s ease-in-out;
}

@keyframes move {
  0% {
    transform: translateX(0);
  }
  100% {
    transform: translateX(calc(100vw - 140px));
  }
}
```

In the example above, the square will return to its initial position after moving to the far right. If we want the square to stay at the far right position after the animation ends, we can use `animation-fill-mode: forwards;`.

```css
.square {
  width: 100px;
  height: 100px;
  background-color: gold;
  animation: move 2s ease-in-out;
  animation-fill-mode: forwards; /* This will keep the last frame of the animation */
}

@keyframes move {
  0% {
    transform: translateX(0);
  }
  100% {
    transform: translateX(calc(100vw - 140px)); /* this style is the last frame, so it will be kept */
  }
}
```

However, please note that 100 keyframes are not always for the last frame. When we use `animation-direction` and `animation-iteration-count`, 100 keyframes may not necessarily be the last frame.

```css
.square {
  width: 100px;
  height: 100px;
  background-color: gold;
  animation: move 2s ease-in-out 2 alternate;
  /*                             ^^^^^^^^^^^ using alternate direction and set to 2 iterations */
  animation-fill-mode: forwards;
}

@keyframes move {
  0% {
    transform: translateX(0); /* now this is the first frame but also the last frame, so it will be kept */
  }
  100% {
    transform: translateX(calc(100vw - 140px));
  }
}
```

The following link contains a table that calculates the percentage at which the last frame occurs based on different combinations of `animation-direction` and `animation-iteration-count`.

https://developer.mozilla.org/en-US/docs/Web/CSS/animation-fill-mode

## Fill mode: Backwards

```css
.square {
  width: 100px;
  height: 100px;
  background-color: gold;
  animation: move 2s 2s ease-in-out;
  /*                 ^^ add delayed start */
}

@keyframes move {
  0% {
    transform: translateX(0);
    background-color: blue;
  }
  100% {
    transform: translateX(calc(100vw - 140px));
  }
}
```

In the example above, we added a 2-second delay, so the animation will start after 2 seconds. This means you will only see the square turn blue and start moving after 2 seconds. However, if we want to see the square turn blue before the animation starts during the delay time, we can use `animation-fill-mode: backwards;`.

```css
.square {
  width: 100px;
  height: 100px;
  background-color: gold;
  animation: move 2s 2s ease-in-out;
  animation-fill-mode: backwards; /* This will apply the styles of the first keyframe before the animation starts */
}

@keyframes move {
  0% {
    transform: translateX(0); /* this style will be applied before the animation starts */
    background-color: blue; /* this style will be applied before the animation starts */
  }
  100% {
    transform: translateX(calc(100vw - 140px));
  }
}
```

The same, 0 keyframe is not always the first frame. When we use `animation-direction` and `animation-iteration-count`, the 0 keyframe may not necessarily be the first frame.

```css
.square {
  width: 100px;
  height: 100px;
  background-color: gold;
  animation: move 2s 2s ease-in-out reverse;
  /*                                ^^^^^^^ this will reverse the animation, so the first frame will be the 100% keyframe */
  animation-fill-mode: backwards;
}

@keyframes move {
  0% {
    transform: translateX(0);
    background-color: blue;
  }
  100% {
    transform: translateX(calc(100vw - 140px)); /* this style is the first frame because of the reverse direction, so it will be applied before the animation starts */
  }
}
```

## Fill mode: Both

```css
.square {
  width: 100px;
  height: 100px;
  background-color: gold;
  animation: move 2s 2s ease-in-out;
  animation-fill-mode: backwards;
}

@keyframes move {
  0% {
    transform: translateX(0);
    background-color: blue;
  }
  100% {
    transform: translateX(calc(100vw - 140px));
  }
}
```

In the example above, we use `animation-fill-mode: backwards;` to make the square turn blue before the animation starts. However, after the animation ends, the square will return to its original gold color and position. If we also want to maintain the style of the last frame, we can use `animation-fill-mode: both;`.

```css
.square {
  width: 100px;
  height: 100px;
  background-color: gold;
  animation: move 2s 2s ease-in-out;
  animation-fill-mode: both;
  /*                   ^^^^ enable both backwards and forwards fill modes */
}

@keyframes move {
  0% {
    transform: translateX(0); /* this style will be applied before the animation starts */
    background-color: blue; /* this style will be applied before the animation starts */
  }
  100% {
    transform: translateX(calc(100vw - 140px)); /* this style will be kept after the animation ends */
  }
}
```
