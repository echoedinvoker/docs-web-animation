---
date: 2025-08-08
type: fact
aliases:
  -
hubs:
  - "[[web-animation]]"
---

# multiple animations

```css
.square {
  width: 100px;
  height: 100px;
  background-color: gold;
  animation: move 2s ease-in-out 2 alternate;
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

Above example animates a square horizontally across the screen. Now we want to add a new animation `bounce` that animates the square vertically while the horizontal animation is running.

```css
.square {
  width: 100px;
  height: 100px;
  background-color: gold;
  animation: move 2s ease-in-out 2 alternate,
    bounce 0.3s ease-in-out infinite alternate; /* add bounce animation */
}

@keyframes move { ... }

/* create a new keyframes for bounce */
@keyframes bounce {
  0% {
    transform: translateY(0);
  }
  100% {
    transform: translateY(100px);
  }
}
```

But this will not work as expected because the `transform` property in both animations will conflict with each other. The last `transform` value will override the previous one. So we only see the `bounce` animation.

To fix this, we can use other properties to achieve the same effect without conflicting with the `transform` property.

```css
@keyframes bounce {
  0% {
    margin-top: 0; /* use margin-top instead of transform */
  }
  100% {
    margin-top: 100px; /* use margin-top instead of transform */
  }
}
```

If we don't use shorthand `animation` property, we can specify multiple animation properties like this:

```css
.square {
  width: 100px;
  height: 100px;
  background-color: gold;
  animation-name: move, bounce;
  animation-duration: 2s, 0.3s;
  animation-timing-function: ease-in-out;
  /*                         ^^^^^^^^^^^ only one value, it applies to both animations */
  animation-iteration-count: 2, infinite;
  animation-direction: alternate;
  /*                   ^^^^^^^^^^^ only one value, it applies to both animations */
}
```
