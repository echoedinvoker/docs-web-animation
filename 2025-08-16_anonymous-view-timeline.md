---
date: 2025-08-16
type: fact
aliases:
  -
hubs:
  - "[web-animation](./web-animation.md)"
---

# anonymous view timeline

## Naming a `view-timeline` and applying an animation based on it

```html
  <body>
    <div class="container">
      <div class="content">
        <div style="height: 200vh; background-color: #2d2337"></div>
        <div class="progress">
          <div class="progress-inner"></div>
        </div>
        <div style="height: 200vh; background-color: #2d2337"></div>
      </div>
    </div>
  </body>
```

```css
body { ... }

.container {
  height: 100vh;
  overflow: auto;
  background-color: #100e1e;
}

.content {
  padding: 30px;
  max-width: 700px;
  margin: 0 auto;
}

.progress {
  width: 100%;
  height: 100px;
  background-color: rgb(57, 41, 67);
  position: relative;
  margin: 20px 0;
}
.progress-inner {
  position: absolute;
  height: 100%;
  top: 0;
  left: 0;
  background-color: rgb(209, 90, 213);
  view-timeline: --progress-view block; /* naming the view timeline */
  animation: animateWidth linear(0, 0.2, 1) both;
  animation-timeline: --progress-view; /* using the named view timeline */
  view-timeline-inset: 10% 20%; /* defining the inset of the edges for the view timeline */
}

@keyframes animateWidth {
  from {
    width: 0;
  }
  to {
    width: 100%;
  }
}
```

## Using `view()` to simplify code above

In the example above, we use `.progress-inner` itself to provide `view-timeline`, and apply animation `animateWidth` based on it. When the object providing `view-timeline` and the object applying the animation are the same, we can simplify the code by directly using `view()`.

```css
.progress-inner {
  position: absolute;
  height: 100%;
  top: 0;
  left: 0;
  background-color: rgb(209, 90, 213);
  /* view-timeline: --progress-view block;   <-- not needed */
  animation: animateWidth linear(0, 0.2, 1) both;
  animation-timeline: view(block 10% 20%);
  /*                  ^^^^^^^^^^^^^^^^^^^ <-- using view() directly (`block` can be omitted) */
}
```

In the above situation, we use `view()` to simplify the code without needing to name the `view-timeline`, so this approach is called `anonymous view timeline`.
