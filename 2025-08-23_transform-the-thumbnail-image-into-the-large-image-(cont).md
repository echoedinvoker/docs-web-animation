---
date: 2025-08-23
type: fact
aliases:
  -
hubs:
  - "[web-animation](./web-animation.md)"
---

# transform the thumbnail image into the large image (cont)

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

    // Set view transition name `image` to the thumbnail image of the old state
    thumbnail.style.viewTransitionName = "image";
    largeImage.style.viewTransitionName = "none";

    const transition = document.startViewTransition(() => {
      // set view transition name `image` to the large image of the new state and remove the view transition name from the thumbnail image
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
    document.startViewTransition(() => {
      displayGrid();
    });
  });
});
```


```css
::view-transition-old(image), ::view-transition-new(image) {
  animation: none;
  mix-blend-mode: normal;
  height: 100%;
  overflow: hidden;
}
```

In the example above, we have implemented a transition animation from thumbnail image to large image, and removed the default crossfade effect. However, when we go back from the large image to the grid view, we will find that the large image will freeze until all transition animations are completed before disappearing. This is because the `view-transition-name: image` of the large image does not have a corresponding element when transitioning back to the grid view, so the browser cannot animate the large image and can only remove it after all transition animations are completed.

## Fix the large image freezing issue when returning to grid view

To solve the above problem, we can remove the `view-transition-name` from the large image after the transition from thumbnail image to large image is completed. This way, when returning to grid view, the large image will be treated as the root for the transition animation, instead of freezing on the screen.


```js
document.addEventListener("DOMContentLoaded", () => {
  // ...

  function expandImage(item) { ... }

  function displayGrid() { ... }

  grid.addEventListener("click", async (e) => {
    // ...

    const thumbnail = item.querySelector("img");
    const largeImage = main.querySelector("img");

    thumbnail.style.viewTransitionName = "image";
    largeImage.style.viewTransitionName = "none";

    const transition = document.startViewTransition(() => {
      thumbnail.style.viewTransitionName = "none";
      largeImage.style.viewTransitionName = "image";
      expandImage(item);
    });

    await transition.updateCallbackDone; // after the DOM update is done

    largeImage.style.viewTransitionName = "none"; // remove the view transition name from the large image

    // ...
  });
  
  // ...
});
```

## Transition from large image back to grid view

If we do not want the large image to be treated as the root for transition animation when returning to grid view, but instead want the large image to smoothly transition back to the corresponding thumbnail image in the grid view, we can set the currently active thumbnail image to `view-transition-name: image` when returning to the grid view, and remove the `view-transition-name` from the large image. This way, the large image can smoothly transition back to the corresponding thumbnail image in the grid view.


```js
document.addEventListener("DOMContentLoaded", () => {
  // ...

  function expandImage(item) { ... }

  function displayGrid() { ... }

  grid.addEventListener("click", async (e) => {
    // ...

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

    // largeImage.style.viewTransitionName = "none";  // no longer needed

    // ...
  });
  
  gridButton.addEventListener("click", async (e) => {
    // ...

    // Get the active thumbnail image and the large image elements
    const activeThumbnail = grid.querySelector(".active img");
    const largeImage = main.querySelector("img");

    document.startViewTransition(() => {
      // set view transition name `image` to the grid item image that is currently active and remove the view transition name from the large image
      activeThumbnail.style.viewTransitionName = "image";
      largeImage.style.viewTransitionName = "none";
      displayGrid();
    });
  });
});
```

The above correction has met our needs, but it will be noticed that when continuing to click on other thumbnails, there is no transition animation from thumbnail image to large image. This is because when clicking on other thumbnails, multiple elements with `view-transition-name: image` are generated, causing the browser to be unable to correctly map to the element to transition to.

## Fix the multiple elements with the same view-transition-name issue

To solve the above problem, we can remove the current thumbnail image's `view-transition-name: image` after completing the transition from large image back to grid view. This way, when clicking on another thumbnail image next time, there won't be multiple elements with `view-transition-name: image` present.

```js
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

    await transition.updateCallbackDone; // wait until the DOM update is done

    activeThumbnail.style.viewTransitionName = "none"; // remove the view transition name from the active thumbnail image to avoid using the same view transition name in the next transition
  });
```
