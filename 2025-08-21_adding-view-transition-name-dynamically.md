---
date: 2025-08-21
type: fact
aliases:
  -
hubs:
  - "[web-animation](./web-animation.md)"
---

# adding view transition name dynamically

## Isolate the grid from the root pseudo-element (already implemented slide in/out transition)

```html
  <body>
    <div class="header"> ... </div>
    <div class="grid-outer">
      <div class="grid"> ... </div>
      <div class="main"> ... </div>
    </div>
  </body>
```

In the above `.main`, there is already a slide in/out transition animation. We want to isolate `.grid` from it.

```css
.grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 20px;
  padding: 20px;
  view-transition-name: grid; /* isolating the grid from the root pseudo-element */
}
```

## Modify the main slide in/out animation 

Because we isolated `.grid` from the root, the slide in/out of the root will move under `.grid`, which looks strange. So below, I slightly adjusted the animation of the slide in/out to make it look more natural.

```css
::view-transition-old(root) {
  animation: 0.5s both ease-in-out slideOut;
}
::view-transition-new(root) {
  animation: 0.5s both ease-in-out slideIn;
}

@keyframes slideOut {
  to {
    transform: translateX(-30px);
    /*                    ^^^^^ replace -100% with -30px to avoid moving under the grid */
    opacity: 0; /* add opacity to fade out to make it look more natural */
  }
}

@keyframes slideIn {
  from {
    transform: translateX(-30px);
    /*                    ^^^^^ replace -100% with -30px to avoid moving under the grid */
    opacity: 0; /* add opacity to fade in to make it look more natural */
  }
  to {
    transform: translateX(0);
    opacity: 1; /* add opacity to fade in to make it look more natural */
  }
}
```

## Prevend the size and position animation when switching between grid and main

We make the `view-transition-name` before and after `.grid` different, so that we can avoid animations for size and position in `::view-transition-group`.

```css
.grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 20px;
  padding: 20px;
  /* view-transition-name: grid;  removed because we want to set this dynamically with JS */
}
```


Set the `view-transition-name` dynamically in JavaScript when expanding an image and displaying the grid again.

```js
document.addEventListener("DOMContentLoaded", () => {
  // ...

  function expandImage(item) {
    // ...

    grid.style.viewTransitionName = "grid"; // Set the view transition name dynamically
  }

  function displayGrid() {
    document.documentElement.scrollTop = 0;

    grid.style.viewTransitionName = "none"; // Set the view transition name to none to prevent size and position animation

    // ...
  }

  // ...
});
