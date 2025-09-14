---
date: 2025-08-23
type: fact
aliases:
  -
hubs:
  - "[web-animation](./web-animation.md)"
---

# animate view transition pseudo elements with the WAAP

```js
document.addEventListener("DOMContentLoaded", () => {
  const grid = document.querySelector(".grid");
  const header = document.querySelector(".header");
  const gridOuter = document.querySelector(".grid-outer");
  const main = document.querySelector(".main .item");
  const gridButton = document.querySelector(".grid-view-button");

  function expandImage(item) {
    const title = item.querySelector("h3").innerText;
    const largeImage = item.dataset.largeImage;
    main.querySelector("h2").innerText = title;
    main.querySelector("img").src = largeImage;
    gridButton.style.display = "block";
    header.classList.add("expanded");
    gridOuter.classList.add("expanded");

    grid
      .querySelectorAll(".active")
      .forEach((e) => e.classList.remove("active"));
    item.classList.add("active");
  }

  function displayGrid() {
    document.documentElement.scrollTop = 0;
    gridButton.style.display = "none";
    gridOuter.classList.remove("expanded");
    header.classList.remove("expanded");
    grid
      .querySelectorAll(".active")
      .forEach((e) => e.classList.remove("active"));
  }

  grid.addEventListener("click", async (e) => {
    const item = e.target.closest(".grid-item");
    if (!item || item.classList.contains("active")) return;

    if (!document.startViewTransition) {
      expandImage(item);
      return;
    }

    const transition = document.startViewTransition(() => {
      expandImage(item);
    });

    await transition.finished;

    item.scrollIntoView({
      behavior: "smooth",
      block: "nearest",
    });
  });

  gridButton.addEventListener("click", async (e) => {
    if (!document.startViewTransition) {
      displayGrid();
      return;
    }

    const transition = document.startViewTransition(() => {
      displayGrid();
    });
  });
});
```


```css
.header {
  background-color: #0c0d12;
  position: sticky;
  top: 0;
  z-index: 100;
  view-transition-name: header;
}
.header .title {
  view-transition-name: header-title;
}
```


In the example above, we isolate only the `.header` and `.header .title` elements from the root of the transition, and each transition part uses the default animation.

## Start with disabling the default crossfade animation and know the `getBoundingClientRect()` method

If we want the new image to expand from the original item's position when expanding the image, we need to use `clip-path` to achieve this, and can only use the `Element.getBoundingClientRect()` method in JavaScript to obtain the element's boundary information, so we must use WAAP to implement this effect.

First, disable the default crossfade animation of the pseudo element root.

```css
::view-transition-old(root), ::view-transition-new(root) {
  animation: none;
  mix-blend-mode: normal;;
}
```

Next, you can use the `Element.getBoundingClientRect()` method to get the boundary information of the clicked element. Below are related documents and diagrams.

[doc of getBoundingClientRect](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/getBoundingClientRect)

![get-bounding-client-rect.png](../assets/imgs/get-bounding-client-rect.png)


```js
document.addEventListener("DOMContentLoaded", () => {
  // ...

  function expandImage(item) { ... }
  function displayGrid() { ... }

  grid.addEventListener("click", async (e) => {
    const item = e.target.closest(".grid-item");
    if (!item || item.classList.contains("active")) return;

    if (!document.startViewTransition) {
      expandImage(item);
      return;
    }

    console.log(item.getBoundingClientRect());  // check the informations we can use

    const transition = document.startViewTransition(() => {
      expandImage(item);
    });

    // ...
  });

  // ...
});
```

```
DOMRect {
    bottom: 381.6666946411133
    height: 281.66668701171875
    left: 840.0000610351562
    right: 1093.3334045410156
    top: 100.00000762939453
    width: 253.33334350585938
    x: 840.0000610351562
    y: 100.00000762939453
}
```

## Implement the clip-path animation with WAAP

After understanding the return value of `getBoundingClientRect()`, we can start using WAAP to create animations for the pseudo-element `::view-transition-new(root)`.

```js
document.addEventListener("DOMContentLoaded", () => {
  // ...

  function expandImage(item) { ... }
  function displayGrid() { ... }

  grid.addEventListener("click", async (e) => {
    const item = e.target.closest(".grid-item");

    // ...

    // get the bounding rectangle information of the clicked item
    const { top, left, right, bottom } = item.getBoundingClientRect();

    const transition = document.startViewTransition(() => {
      expandImage(item);
    });

    await transition.ready; // make sure our animation creation code runs after the pseudo elements are created

    // create animation on the ::view-transition-new(root) pseudo element
    document.documentElement.animate([
      {
        clipPath: `inset(${top}px ${innerWidth - right}px ${innerHeight - bottom}px ${left}px)`,
      },
      { clipPath: "inset(0%)" }
    ], {
      duration: 300,
      easing: "ease-in",
      pseudoElement: "::view-transition-new(root)"
    });

    // ...
  });

  // ...
});
```

## Add a certain filter effect to make the animation more noticeable

When selecting different items in the expanded image state, because the items on the left side do not change, the animation effect on the left side will be very subtle. Therefore, in addition to clip-path, we can add some other effects to make the animation more noticeable.


```js
    document.documentElement.animate([
      {
        clipPath: `inset(${top}px ${innerWidth - right}px ${innerHeight - bottom}px ${left}px)`,
        filter: "contrast(0.3)", // add a certain filter effect to make the animation more clear
      },
      {
        clipPath: "inset(0%)",
        filter: "contrast(1)", // add a certain filter effect to make the animation more clear
      }
    ], {
      duration: 300,
      easing: "ease-in",
      pseudoElement: "::view-transition-new(root)"
    });
```


## Get back the crossfade animation when returning to grid view from the expanded image

Because we removed the default animations for `::view-transition-new(root)` and `::view-transition-old(root)`, there will be no animation effect when returning from the expanded image to the grid view. We can temporarily add back the default crossfade animation in this situation using the following method.


```js
document.addEventListener("DOMContentLoaded", () => {
  // ...

  function expandImage(item) { ... }
  function displayGrid() { ... }

  grid.addEventListener("click", async (e) => { ... });

  gridButton.addEventListener("click", async (e) => {
    // ...

    // before starting the transition, we add a temporary class to the root element
    document.documentElement.classList.add("back");

    const transition = document.startViewTransition(() => {
      displayGrid();
    });

    // after the transition is finished, we remove the temporary class
    await transition.finished;
    document.documentElement.classList.remove("back");
  });
});
```

Then add `html:not(.back)` to the selectors in the CSS that originally disabled crossfade, to exclude it when returning to grid view.

```css
html:not(.back)::view-transition-old(root),
html:not(.back)::view-transition-new(root) {
  animation: none;
  mix-blend-mode: normal;;
}
```
