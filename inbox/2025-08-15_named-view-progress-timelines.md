---
date: 2025-08-15
type: fact
aliases:
  -
hubs:
  - "[[web-animation]]"
---

# named view progress timelines

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
...

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
  width: 50%;
  height: 100%;
  top: 0;
  left: 0;
  background-color: rgb(209, 90, 213);
}
```

## Named View Progress Timelines

Assuming we want to set the animation timeline based on the entering and leaving viewport status of `.progress-inner`, it can be set as follows:

```css
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
  view-timeline: --progress-view block; /* provides a subject for the animation timeline (in this case, itself) */
  animation: animateWidth linear both; /* animates the width from 0 to 100% */
  animation-timeline: --progress-view; /* specifies the timeline to use a subject view progress (in this case, itself) */
}

/* create a animation that animates the width from 0 to 100% */
@keyframes animateWidth {
  from {
    width: 0;
  }
  to {
    width: 100%;
  }
}
```


```css
.progress-inner {
  position: absolute;
  height: 100%;
  top: 0;
  left: 0;
  background-color: rgb(209, 90, 213);
  view-timeline: --progress-view block;
  animation: animateWidth linear(0, 0.2, 1) both;
  /*                      ^^^^^^^^^^^^^^^^^ when the subject is in the middle of the viewport, the animation progresses at 20% */
  animation-timeline: --progress-view;
}
```

## Scope issue of the view timeline

```css
.container {
  height: 100vh;
  overflow: auto;
  background-color: #100e1e;
  animation: changeBg linear both; /* applies the animation to the container */
  animation-timeline: --progress-view; /* uses the named view timeline from the `.progress-inner` element */
}
.content { ... }
.progress { ... }
.progress-inner {
  position: absolute;
  height: 100%;
  top: 0;
  left: 0;
  background-color: rgb(209, 90, 213);
  view-timeline: --progress-view block; /* targeted timeline (subject) */
  animation: animateWidth linear(0, 0.2, 1) both;
  animation-timeline: --progress-view;
}

@keyframes animateWidth { ... }

/* create a animation for the container that changes the background color */
@keyframes changeBg {
  to {
    background-color: black;
  }
}
```

Above we created an animation for the `.container` that uses the `--progress-view` timeline. But not working as expected, because the `view-timeline` is set to the `.progress-inner` element, which is inside the `.container`, but not the subject of the view timeline.


```html
  <body>
    <div class="container"> <!-- the container is NOT inside the subject -->
      <div class="content">
        <div style="height: 200vh; background-color: #2d2337"></div>
        <div class="progress">
          <div class="progress-inner"></div> <!-- this is the subject of the view timeline -->
        </div>
        <div style="height: 200vh; background-color: #2d2337"></div>
      </div>
    </div>
  </body>
```

```css
body {
  ...
  timeline-scope: --progress-view; /* expends the timeline scope to the body element */
}
```

Now the `changeBg` animation should work as expected.

## Animation Range

Just like `scroll progress timelines`, you can also use `animation-range` to limit the animation to work within a specific range of the viewport.

```css
.container {
  height: 100vh;
  overflow: auto;
  background-color: #100e1e;
  animation: changeBg linear both;
  animation-timeline: --progress-view;
  animation-range: 30% 70%; /* this means the animation will only run when the subject into the viewport is between 30% and 70% */
}
```
