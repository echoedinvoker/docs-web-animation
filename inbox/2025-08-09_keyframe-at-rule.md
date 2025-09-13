---
date: 2025-08-09
type: fact
aliases:
  -
hubs:
  - "[[web-animation]]"
---

# keyframe at rule

## Animation cascading or not?

### same keyframe name of different animation (no cascading)

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

@keyframes move {
/*         ^^^^ same name as above animation */
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}
```

In the example above, there are two animations both named `move`, this will cause the second `@keyframes` to override the first one.

### same keyframe percentage of different properties (has cascading)

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
  50% {
    transform: translateX(300px);
  }
  50% { /* multiple keyframes percentages but with different properties */
    background-color: blue;
  }
  100% {
    transform: translateX(calc(100vw - 140px));
  }
}
```

In the example above, there are two keyframes of 50% setting different properties. This writing style is legal, and the browser will automatically cascade the properties.

## !important to the animation properties

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
  50% {
    transform: translateX(300px);
  }
  50% {
    background-color: blue !important;
    /*                     ^^^^^^^^^^ if we add !important here, this property will be entirely ignored */
  }
  100% {
    transform: translateX(calc(100vw - 140px));
  }
}
```

## Animation composition

### simple color addition

```css
.square {
  width: 100px;
  height: 100px;
  background-color: gold; /* initial background color */
  animation: move 2s ease-in-out;
  animation-composition: add; /* set the animation composition to add */
}

@keyframes move {
  0% {
    transform: translateX(0);
  }
  50% {
    transform: translateX(300px);
  }
  50% {
    background-color: blue; /* this will be added with the initial background color, resulting in a blend of colors */
  }
  100% {
    transform: translateX(calc(100vw - 140px));
  }
}
```

### Composition with different values

```css
.square {
  width: 100px;
  height: 100px;
  background-color: gold; /* initial background color */
  animation: move 2s ease-in-out;
  animation-composition: add;
  transform: translateX(50px); /* initial transform */
}

@keyframes move {
  0% {
    transform: translateX(0); /* this will be added to the initial styles, resulting `background-color: gold translateX(50px) translateX(0)` */
                              /*                                                                                ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ move twice */
  }
  50% {
    transform: translateX(300px);  /* this will be added to the initial styles, resulting `background-color: gold translateX(50px) translateX(300px)` */
  }
  50% {
    background-color: blue; /* this will be added with the initial styles, resulting `background-color: (blend of gold and blue) translateX(50px) translateX(300px)` */
  }
  100% {
    transform: translateX(calc(100vw - 140px)); /* this will be added to the initial styles, resulting `background-color: (blend of gold and blue) translateX(50px) translateX(calc(100vw - 140px))` */
  }
}
```

If you want only the `transform: translateX(300px)` to be affected by `animation-composition: add` at 50%, while the background-color remains unaffected and appears blue, what should you do?

```css
.square {
  width: 100px;
  height: 100px;
  background-color: gold;
  animation: move 2s ease-in-out;
  animation-composition: add;
  transform: translateX(50px);
}

@keyframes move {
  0% {
    transform: translateX(0);
  }
  50% {
    transform: translateX(300px);
  }
  50% {
    background-color: blue;
    animation-composition: replace; /* add this line to let this block use the replace composition instead of add */
  }
  100% {
    transform: translateX(calc(100vw - 140px));
  }
}
```

## Question!

```css
.square {
  width: 100px;
  height: 100px;
  background-color: gold;
  animation: move 2s ease-in-out;
  animation-composition: add;
  transform: translateX(50px);
}

@keyframes move {
  0% {
    transform: translateX(0);
  }
  25% {
    background-color: red; /* here we add a red background at 25%, what will happen? */
  }
  50% {
    transform: translateX(300px);
  }
  50% {
    background-color: blue;
    animation-composition: replace;
  }
  100% {
    transform: translateX(calc(100vw - 140px));
  }
}
```

1. At 25%, even if we only define `background-color: red`, `transform: translateX` will automatically interpolate to get the appropriate value, so we don't need to write it manually.
2. The `background-color: red` of 25% will mix with the initial `background-color: gold` to form a new color. However, the `background-color: blue` of 50%, because it has set `animation-composition: replace`, will display a complete blue color without mixing with the initial `background-color: gold`.
