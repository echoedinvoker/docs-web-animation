# remove transition for users who prefers reduced motion

```js
document.addEventListener("DOMContentLoaded", () => {
  const grid = document.querySelector(".grid");
  const header = document.querySelector(".header");
  const gridOuter = document.querySelector(".grid-outer");
  const main = document.querySelector(".main .item");
  const gridButton = document.querySelector(".grid-view-button");

  function populateGridItemViewTransitionNames(clear = false) {
    grid.querySelectorAll(".grid-item").forEach((item, index) => {
      item.style.viewTransitionName = clear ? "none" : `grid-item-${index}`;
    });
  }

  populateGridItemViewTransitionNames();

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

    populateGridItemViewTransitionNames(true);

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

    populateGridItemViewTransitionNames();

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

In the example above, we can see that we have used many transitions, but if the user enables prefers-reduced-motion, we must actively remove these transitions to avoid making the user uncomfortable.

Removing transitions is very simple, we can use CSS media query `prefers-reduced-motion`, and disable the animations of the pseudo-elements `::view-transition-group`, `::view-transition-old`, and `::view-transition-new`.

```css
@media (prefers-reduced-motion) {
  ::view-transition-group(*),
  ::view-transition-old(*),
  ::view-transition-new(*) {
    animation: none !important;
  }
}
```
