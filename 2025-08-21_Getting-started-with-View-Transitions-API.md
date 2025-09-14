---
date: 2025-08-21
type: fact
aliases:
  -
hubs:
  - "[web-animation](./web-animation.md)"
---

# Getting started with View Transitions API

```html
  <body>
    <div class="header">
      <div class="header-inner">
        <button class="grid-view-button" aria-label="Grid view">
          <img src="./assets/grid-svgrepo-com.svg" alt="" />
        </button>
        <div class="title">
          <h1>Echoes of What Was <span></span></h1>
        </div>
      </div>
    </div>
    <div class="grid-outer">
      <div class="grid">
        <div
          class="grid-item"
          tabindex="0"
          data-large-image="./assets/img-1-lg.jpg"
        >
          <img src="./assets/img-1-th.jpg" alt="" />
          <h3>Forgotten Dreams</h3>
        </div>
        <div
          class="grid-item"
          tabindex="0"
          data-large-image="./assets/img-2-lg.jpg"
        >
          <img src="./assets/img-2-th.jpg" alt="" />
          <h3>The Weight of the World</h3>
        </div>
        <div
          class="grid-item"
          tabindex="0"
          data-large-image="./assets/img-3-lg.jpg"
        >
          <img src="./assets/img-3-th.jpg" alt="" />
          <h3>Aching Heart</h3>
        </div>
        <div
          class="grid-item"
          tabindex="0"
          data-large-image="./assets/img-4-lg.jpg"
        >
          <img src="./assets/img-4-th.jpg" alt="" />
          <h3>Lost and Alone</h3>
        </div>
        <div
          class="grid-item"
          tabindex="0"
          data-large-image="./assets/img-5-lg.jpg"
        >
          <img src="./assets/img-5-th.jpg" alt="" />
          <h3>A Glimpse of Light</h3>
        </div>
        <div
          class="grid-item"
          tabindex="0"
          data-large-image="./assets/img-6-lg.jpg"
        >
          <img src="./assets/img-6-th.jpg" alt="" />
          <h3>The Empty Chair</h3>
        </div>
        <div
          class="grid-item"
          tabindex="0"
          data-large-image="./assets/img-7-lg.jpg"
        >
          <img src="./assets/img-7-th.jpg" alt="" />
          <h3>Healing Begins</h3>
        </div>
        <div
          class="grid-item"
          tabindex="0"
          data-large-image="./assets/img-8-lg.jpg"
        >
          <img src="./assets/img-8-th.jpg" alt="" />
          <h3>The Last Goodbye</h3>
        </div>
      </div>
      <div class="main">
        <div class="item">
          <img src="./assets/img-1-lg.jpg" alt="" />
          <h2>Forgotten Dreams</h2>
          <div class="info">
            <div class="date info-item">
              <img src="./assets/date-range-svgrepo-com.svg" alt="" />
              <span>19 November 2019</span>
            </div>
            <div class="camera info-item">
              <img src="./assets/camera-svgrepo-com.svg" alt="" />
              <span>Sony Alpha 8</span>
            </div>
          </div>
          <div class="description">
            <p>
              The house creaked with an unsettling silence, a stark contrast to
              the joyous chaos that once filled it. Dust motes danced in the
              pale slivers of sunlight filtering through the blinds,
              illuminating the empty space where his favorite chair used to be.
              The worn leather, once a testament to countless evenings spent
              reading and sharing stories, now gaped like a missing tooth, a
              constant reminder of the gaping hole left in their lives.
            </p>
          </div>
        </div>
      </div>
    </div>
  </body>
```

```css
body {
  background-color: var(--bg);
  color: #fff;
  font-family: "Source Code Pro", monospace;
  font-optical-sizing: auto;
  padding: 0;
  margin: 0;
}
.visually-hidden {
  clip: rect(0 0 0 0);
  clip-path: inset(50%);
  height: 1px;
  overflow: hidden;
  position: absolute;
  white-space: nowrap;
  width: 1px;
}
img {
  max-width: 100%;
}

* {
  box-sizing: border-box;
}
:root {
  --bg: #12141a;
  --header-height: 80px;
}

.container {
  width: 100%;
  padding: 0 20px;
  max-width: 1200px;
  margin: 0 auto;
}
.header {
  background-color: #0c0d12;
  position: sticky;
  top: 0;
  z-index: 100;
}
.header .header-inner {
  height: var(--header-height);
  display: flex;
  padding: 0 20px;
  justify-content: center;
  align-items: center;
}
.header.expanded .header-inner {
  justify-content: flex-start;
}

.header .title h1 {
  color: #c7d0e9;
  font-weight: 400;
}
.header .title h1 span {
  font-size: 1.7rem;
  color: #8696c8;
}
.header .grid-view-button {
  border: none;
  padding: 0;
  margin: 0;
  background: transparent;
  margin-right: 20px;
  cursor: pointer;
  display: none;
}
.header .grid-view-button img {
  width: 28px;
  vertical-align: middle;
}
.grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 20px;
  padding: 20px;
}

.grid .grid-item {
  border: 1px solid #4f5a7a;
  border-radius: 3px;
  padding: 10px;
  cursor: pointer;
  transition: border-color 0.3s;
}
.grid .grid-item.active {
  border-color: #6ec7d5;
}
.grid .grid-item:not(.active):hover {
  border-color: #8696c8;
}
.grid .grid-item:focus-visible {
  outline: 3px solid #65ece8;
}
.grid .grid-item:hover img,
.grid .grid-item:focus img {
  filter: saturate(1);
}
.grid .grid-item h3 {
  font-size: 1.5rem;
  font-weight: 400;
  margin: 0;
  padding: 20px 0;
  line-height: 1.6;
}
.grid .grid-item img {
  filter: saturate(0.4);
  aspect-ratio: 1.666;
  object-fit: cover;
  transition: 0.3s;
}

.grid-outer.expanded {
  display: flex;
}
.grid-outer.expanded .grid {
  height: calc(100vh - var(--header-height));
  overflow: auto;
  width: 300px;
  flex-grow: 200px;
  grid-template-columns: repeat(1, 1fr);
  position: sticky;
  top: var(--header-height);
}
.grid-outer.expanded .grid .grid-item h3 {
  font-size: 1.1rem;
  padding: 10px 0;
}
.grid-outer .main {
  position: sticky;
  top: var(--header-height);
  padding: 20px;
  flex: 1;
  max-width: 800px;
  display: none;
}
.grid-outer.expanded .main {
  display: block;
}
.grid-outer .main h2 {
  font-size: 2rem;
  font-weight: 500;
  letter-spacing: 2px;
}
.grid-outer .main .info .info-item {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}
.grid-outer .main .info .info-item img {
  width: 24px;
  margin-right: 10px;
}
.grid-outer .main .description p {
  line-height: 1.8;
  margin-top: 40px;
}
```

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
    expandImage(item);
  });
  
  gridButton.addEventListener("click", async (e) => {
    displayGrid();
  });
});
```

## Applying View Transitions when switching between grid and main sections

We just need to use the `document.startViewTransition()` method to wrap the function calls that modify the DOM.

First, analyze the code where to modify DOM elements when switching between the grid and main sections.

Second, wrap the function calls in `document.startViewTransition()` to apply the view transition effect.

```js
document.addEventListener("DOMContentLoaded", () => {
  //...

  function expandImage(item) { ... } // function to expand the main section to show the image details

  function displayGrid() { ... } // function to back to the grid view

  grid.addEventListener("click", async (e) => {
    //...

    // wrap the expandImage call in a view transition
    document.startViewTransition(() => {
      expandImage(item);
    });
  });
  
  gridButton.addEventListener("click", async (e) => {

    // wrap the displayGrid call in a view transition
    document.startViewTransition(() => {
      displayGrid();
    });
  });
});
```

```js

  // ...
  grid.addEventListener("click", async (e) => {
    // ...

    // Check if the View Transitions API is supported, if not, fallback to the original function calling
    if (!document.startViewTransition) {
      expandImage(item);
      return;
    }

    document.startViewTransition(() => {
      expandImage(item);
    });
  });
  
  gridButton.addEventListener("click", async (e) => {
    // Check if the View Transitions API is supported, if not, fallback to the original function calling
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

## What actually `startViewTransition` does?

```
devtool->three dots->More tools->animations->Pause->Click on the grid item
```

Then you can see that there is a psuedo-element `::view-transition` under the `html` element, which contains two `::view-transition-old` and `::view-transition-new` elements that represent the screenshops of the old and new states respectively.

```html
<html ...>
  ::view-transition
    ::view-transition-group(root) <-- responsible for changing the size and the position of the elements
      ::view-transition-image-pair(root)
        ::view-transition-old(root) <-- screenshot of the old state
        ::view-transition-new(root) <-- screenshot of the new state
```

If we check the `::view-transition-old` and `::view-transition-new` elements, we can see that they have two animations applied to them by default:


```
html::view-transition-old(root) {
    animation-name:
        -ua-view-transition-fade-out,
        -ua-mix-blend-mode-plus-lighter;
}
html::view-transition-new(root) {
    animation-name:
        -ua-view-transition-fade-in,
        -ua-mix-blend-mode-plus-lighter;
}
```

> The pseudo-element above is generated by the browser itself, but it is also just a regular DOM element, so we can modify its default animation through CSS.


## Asynchronous View Transitions

`document.startViewTransition()` can accept an asynchronous callback, which allows you to perform actions before the new state is captured (transition only happens after the new state is captured). 

```js
    document.startViewTransition(async () => {
    //                           ^^^^^ the callback of startViewTransition can be asynchronous, it lets you can do things before the new state is captured

      await new Promise((resolve) => setTimeout(resolve, 1000)); // this should be done before the new state is captured
      displayGrid();
    });


