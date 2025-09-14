# Use The Same View Transition Name on The Different Elements

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

    await transition.updateCallbackDone;

    item.scrollIntoView({
      behavior: "smooth",
      block: "nearest"
    })
  });
  
  // ...
});
```

## Transition between different elements by setting the same view transition name on them

We want to create a transition animation when selecting an item in the grid, transitioning from the image of the item to the image in the main section. However, these two images are completely different elements. Even so, we can still give the same view transition name to different elements before and after the animation to achieve this effect.


```js
document.addEventListener("DOMContentLoaded", () => {
  // ...

  grid.addEventListener("click", async (e) => {
    const item = e.target.closest(".grid-item");
    
    // ...

    const thumbnail = item.querySelector("img"); 
    const largeImage = main.querySelector("img");

    thumbnail.style.viewTransitionName = "image"; // name the view transition `image` of the start state
    largeImage.style.viewTransitionName = "none"; // make sure the large image does not have the same view transition name at this point

    const transition = document.startViewTransition(() => {
      thumbnail.style.viewTransitionName = "none"; // remove the view transition name `image`
      largeImage.style.viewTransitionName = "image"; // name the view transition `image` of the end state
      expandImage(item);
    });

    // ...
  });
  
  // ...
});
```


## Remove crossfade animation and only present the `::view-transition-new(image)` durring the transition

During the transition animation, it is clear that the item image and main image overlap and move together, but they are not perfectly matched because of slight differences in aspect ratio. We can disable the crossfade animation of `::view-transition-old(image)` and `::view-transition-new(image)` to make the transition start with the appearance of `::view-transition-new(image)`, so there will be no issue of mismatched overlapping images.


```css
/* remove crossfade animation */
::view-transition-old(image), ::view-transition-new(image) {
  animation: none;
  mix-blend-mode: normal;
}
```


## Prevent distortion of `::view-transition-new(image)` during size and position animation

The size and position animation of `::view-transition-group(image)` will initially compress the height of `::view-transition-new(image)` image, making it look a bit strange. This is because the height of `::view-transition-old(image)` is smaller. We can set it up in the following way to make the height of `::view-transition-new(image)` image fill the height of `::view-transition-group(image)` without distortion.


```
::view-transition
    ::view-transition-group(image) <-- size and position animation still works
        ::view-transition-image-pair(image)
            ::view-transition-old(image)
            ::view-transition-new(image) <-- this will overlap the old image
```


```css
::view-transition-old(image), ::view-transition-new(image) {
  animation: none;
  mix-blend-mode: normal;

  /* fill the height of the view-transition-group and crop the overflow */
  height: 100%;
  overflow: hidden;
}

/* make sure the image covers the container without distortion */
::view-transition-new(image) {
  object-fit: cover;
}
```



