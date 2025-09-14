---
date: 2025-08-23
type: fact
aliases:
  -
hubs:
  - "[web-animation](./web-animation.md)"
---

# animate grid items with dynamic view transition names

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

    grid.style.viewTransitionName = "grid";
  }

  function displayGrid() {
    document.documentElement.scrollTop = 0;

    grid.style.viewTransitionName = "none";

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

    const thumbnail = item.querySelector("img");
    const largeImage = main.querySelector("img");

    thumbnail.style.viewTransitionName = "image";
    largeImage.style.viewTransitionName = "none";

    const transition = document.startViewTransition(() => {
      thumbnail.style.viewTransitionName = "none";
      largeImage.style.viewTransitionName = "image";
      expandImage(item);
    });

    await transition.updateCallbackDone;

    item.scrollIntoView({
      behavior: "smooth",
      block: "nearest"
    })
  });
  
  gridButton.addEventListener("click", async (e) => {

    if (!document.startViewTransition) {
      displayGrid();
      return;
    }

    const activeThumbnail = grid.querySelector(".active img");
    const largeImage = main.querySelector("img");

    const transition = document.startViewTransition(() => {
      activeThumbnail.style.viewTransitionName = "image";
      largeImage.style.viewTransitionName = "none";
      displayGrid();
    });

    await transition.updateCallbackDone;

    activeThumbnail.style.viewTransitionName = "none";
  });
});
```


In the example above, if a grid item is clicked in grid view, only the clicked grid item will transition to a large image, while the other grid items will be treated as part of the root and use the default crossfade transition effect.

## Populating unique view transition names for each grid item

We can simply use JS to give each grid item a unique `view-transition-name`, so that when switching from grid view to expand image or vice versa, other grid items that have not been clicked will also have a positional animation effect.

```js
document.addEventListener("DOMContentLoaded", () => {
  // ...

  // create a function to populate UNIQUE view transition names for each grid item or remove them all
  function populateGridItemViewTransitionNames(clear = false) {
    grid.querySelectorAll(".grid-item").forEach((item, index) => {
      item.style.viewTransitionName = clear ? "none" : `grid-item-${index}`;
    });
  }

  // populate view transition names for grid items when page loads
  populateGridItemViewTransitionNames();

  function expandImage(item) { ... }
  function displayGrid() { ... }

  grid.addEventListener("click", async (e) => { ... });
  gridButton.addEventListener("click", async (e) => { ... });
});

```


Above, when we load the page, we assign a unique `view-transition-name` to each grid item, isolating each grid item from the root. However, when selecting different grid items in the expanded image state, there may be a slight flicker issue because each grid item is isolated individually. In fact, we do not need to isolate them in this operation.

## Fixing flicker issues when switching between different grid items in expanded image state

To solve the above problem, we can remove the `view-transition-name` from all grid items after the transition of expanding the image is completed, allowing them to return to a part of the root. This way, there will be no flickering issues when switching between different grid items in the expanded image state.


```js
document.addEventListener("DOMContentLoaded", () => {
  // ...

  function populateGridItemViewTransitionNames(clear = false) {
    grid.querySelectorAll(".grid-item").forEach((item, index) => {
      item.style.viewTransitionName = clear ? "none" : `grid-item-${index}`;
    });
  }

  populateGridItemViewTransitionNames();

  function expandImage(item) { ... }
  function displayGrid() { ... }

  grid.addEventListener("click", async (e) => {
    // ...

    await transition.updateCallbackDone; // wait for the update callback to finish

    populateGridItemViewTransitionNames(true); // clear view transition names of all grid items

    // ...
  });
  
  gridButton.addEventListener("click", async (e) => {
    // ...

    // populate view transition names again for grid items when going back to grid view
    populateGridItemViewTransitionNames();

    const transition = document.startViewTransition(() => {
      activeThumbnail.style.viewTransitionName = "image";
      largeImage.style.viewTransitionName = "none";
      displayGrid();
    });

    // ...
  });
});
```

## Removing crossfade animations for each grid item

In addition to the position animation of `view-transition-group`, there are also crossfade animations for `view-transition-old` and `view-transition-new` when the grid items above transition. In this case, it is not a problem, but if you want to avoid crossfade, you can make some adjustments to these pseudo-elements in CSS as follows:

The following example assumes that we can use wildcards in selectors, but currently browsers do not support this syntax. It may be supported in the future.

```css
::view-transition-old(grid-item-*), ::view-transition-new(grid-item-*) {
/*                    ^^^^^^^^^^^ wildcard is NOT supported yet! */
  animation: none;
  mix-blend-mode: normal;
}
```

The way that can currently be implemented are as follows:

```css
.grid .grid-item {
  border: 1px solid #4f5a7a;
  border-radius: 3px;
  padding: 10px;
  cursor: pointer;
  transition: border-color 0.3s;
  view-transition-class: grid-item; /* give a class name for its corresponding pseudo-elements */
}

::view-transition-old(*.grid-item), ::view-transition-new(*.grid-item) {
  animation: none;
  mix-blend-mode: normal;
}
```



