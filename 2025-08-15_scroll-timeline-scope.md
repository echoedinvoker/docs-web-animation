---
date: 2025-08-15
type: fact
aliases:
  -
hubs:
  - "[web-animation](./web-animation.md)"
---

# scroll timeline scope

## Expanding the scope of a scroll timeline to the body

```html
  <body>
    <div class="square"></div> <!-- square element is not child of .container, but it uses the scroll timeline of .container -->
    <div class="container"> <!-- scroll timeline is defined on .container -->
      <div class="content">
        <div style="height: 200vh; background-color: black"></div>
      </div>
    </div>
  </body>
```

```css
...
.container {
  height: 100vh;
  overflow: auto;
  background-color: #100e1e;
  scroll-timeline: --container-timeline block; /* named scroll timeline specified by .square */
}

.content {
  padding: 30px;
  max-width: 700px;
  margin: 0 auto;
}

.square {
  width: 100px;
  height: 100px;
  background-color: gold;
  animation: linear move;
  animation-timeline: --container-timeline; /* use the named scroll timeline of `.container` */
  position: sticky;
  top: 0;
}

@keyframes move {
  to {
    transform: translateX(calc(100vw - 100px));
  }
}
```

In the example above, the animation of `.square` will not have any effect because `.square` is not a child element of `.container`, so it cannot use the scroll timeline of `.container`.

But we can use `timeline-scope` to expand the scope of the scroll timeline to the body, so that `.square` can use it.


```css
body {
  ...
  timeline-scope: --container-timeline; /* this expands the scope of `--container-timeline` to the body */
}

.container {
  ...
  scroll-timeline: --container-timeline block; /* be expanded to the body */
}

.content {
  padding: 30px;
  max-width: 700px;
  margin: 0 auto;
}

.square {
  width: 100px;
  height: 100px;
  background-color: gold;
  animation: linear move;
  animation-timeline: --container-timeline; /* now it can use the scroll timeline of the body */
  position: sticky;                         /* and in fact, it comes from the `.container` */
  top: 0;
}

@keyframes move {
  to {
    transform: translateX(calc(100vw - 100px));
  }
}
```

## `timeline-scope` cannot only be used on the body, but also on any element.

```html
  <body>
    <div style="display: flex;">
      <div class="container">
        <div class="square"></div> <!-- this element want to use the scroll timeline from `.container-2`, but not inside it -->
        <div class="content">
          <div style="height: 200vh; background-color: black"></div>
        </div>
      </div>
      <div class="container-2">
        <div class="content">
          <div style="height: 200vh; background-color: black"></div>
        </div>
      </div>
    </div>
  </body>
```


```css
.container {
  flex: 1;
  height: 100vh;
  overflow: auto;
  background-color: #100e1e;
  scroll-timeline: --container-timeline block;
}

.container-2 {
  flex: 1;
  height: 100vh;
  overflow: auto;
  background-color: #487c55;
  scroll-timeline: --container-2 block;
}

.content {
  padding: 30px;
  max-width: 700px;
  margin: 0 auto;
}

.square {
  width: 100px;
  height: 100px;
  background-color: gold;
  animation: linear move;
  animation-timeline: --container-2;
  /*                  ^^^^^^^^^^^^^ cannot get the scroll timeline of `.container-2` because it is not inside it */

  position: sticky;
  top: 0;
}

@keyframes move {
  to {
    transform: translateX(calc(50vw - 100px));
    /*                         ^^^^ */
  }
}
```

Luckily, we can use `timeline-scope` to expand the scope of `--container-2` to the parent element of `.square`. And then, the `.square` can use the scroll timeline of `.container-2` from it.

```html
  <body>
    <div style="display: flex; timeline-scope: --container-2;">
    <!--                       ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ expends the scope of `--container-2` to the here -->
      <div class="container">
        <div class="square"></div>
        <div class="content">
          <div style="height: 200vh; background-color: black"></div>
        </div>
      </div>
      <div class="container-2">
        <div class="content">
          <div style="height: 200vh; background-color: black"></div>
        </div>
      </div>
    </div>
  </body>
```


```css
.square {
  width: 100px;
  height: 100px;
  background-color: gold;
  animation: linear move;
  animation-timeline: --container-2;
  /*                  ^^^^^^^^^^^^^ now it uses the scroll timeline of `.container-2` */
  position: sticky;
  top: 0;
}
```
