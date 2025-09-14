---
date: 2025-08-15
type: fact
aliases:
  -
hubs:
  - "[web-animation](./web-animation.md)"
---

# Overview of scroll timeline and preparation

## What is scroll timeline?

By default, `document.timeline` is based on the time elapsed since the page was loaded. However, in some cases, it is more useful to base animations on the scroll position of a page. So in addition to the default `document.timeline`, we also have scroll based timelines where we can control the animation progress based on the scroll position of the page.

## Two types of scroll timelines

### scroll progress timeline

`scroll progress timeline` is a timeline that maps the scroll position of a page to the progress of an animation. 

If the top of the content is in the viewport (or container), the animation progress is 0, and when the bottom of the content is at the bottom of the viewport (or container), the animation progress is 100.

![scroll-progress-timeline-0.png](../assets/imgs/scroll-progress-timeline-0.png)

![scroll-progress-timeline-100.png](../assets/imgs/scroll-progress-timeline-100.png)

### view progress timeline

`view progress timeline` is similar to `scroll progress timeline`, but it calculates the animation progress based on the visibility of a certain subject rather than page content.

![view-progress-tl-0-subject-out.png](../assets/imgs/view-progress-tl-0-subject-out.png)

![view-progress-tl-0-start.png](../assets/imgs/view-progress-tl-0-start.png)

![view-progress-tl-100.png](../assets/imgs/view-progress-tl-100.png)

## Scroll anmimation debugger (chrome plugin)

Install the plugin to debug scroll animations in Chrome.

https://chromewebstore.google.com/detail/scroll-driven-animations/ojihehfngalmpghicjgbfdmloiifhoce

## Preparation codes

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="stylesheet" href="styles.css" />
    <title>Scroll Driven Animations</title>
  </head>
  <body>
    <div class="container">
      <div class="square"></div> <!-- sticky square with CSS animation -->
      <div class="content">
        <div style="height: 200vh; background-color: black"></div> <!-- overflow content to enable scrolling -->
      </div>
    </div>
  </body>
</html>
```

```css
/* .. */

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

.square {
  width: 100px;
  height: 100px;
  background-color: gold;
  animation: 5s infinite alternate move;
  position: sticky;
  top: 0;
}

@keyframes move {
  to {
    transform: translateX(calc(100vw - 100px));
  }
}
```

In the example above, the sticky square is animated based on the timeline. Next, we will change it to be animated based on the scroll position of the `content`.
