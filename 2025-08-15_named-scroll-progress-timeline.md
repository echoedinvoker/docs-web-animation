---
date: 2025-08-15
type: fact
aliases:
  -
hubs:
  - "[web-animation](./web-animation.md)"
---

# named scroll progress timeline

## Introduction of named scroll progress timeline

```html
  <body>
    <div class="container"> <!-- scroll container -->
      <div class="square"></div> <!-- animated square -->
      <div class="content">
        <div style="height: 200vh; background-color: black"></div>
      </div>
    </div>
  </body>
```

If we want the animation of the square to be determined by the scrolling progress of the container, we can use a named scroll progress timeline.

```css
...
.container {
  height: 100vh;
  overflow: auto;
  background-color: #100e1e;
  scroll-timeline-name: --container-timeline; /* naming the scroll timeline for .square animation to use */
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
  animation: move; /* duration, iteration and delay are meaningless when using scroll timelines */
  animation-timeline: --container-timeline; /* using the named scroll timeline instead of default timeline */
  position: sticky;
  top: 0;
}

/* now the progress of this animation will be based on the scroll progress of .container */
@keyframes move {
  to {
    transform: translateX(calc(100vw - 100px));
  }
}
```

## Use timing functions with scroll timelines

Although the duration, iteration, and delay of animations are meaningless when using scroll timelines, the timing function can still be used.

```css
.square {
  width: 100px;
  height: 100px;
  background-color: gold;
  animation: linear(0, 0.25, 1) move;
  /*         ^^^^^^^^^^^^^^^^^^ timing function is still usable when using scroll timelines */
  animation-timeline: --container-timeline;
  position: sticky;
  top: 0;
}
```

In the example above, when the scroll reaches the middle of the content, you will find that the animation progress of the square is only at 25%, this is because we used the timing function `linear(0, 0.25, 1)`

## Scroll timeline axis and shorthand

We can define the axis of the named scroll progress timeline.

```css
.container {
  height: 100vh;
  overflow: auto;
  background-color: #100e1e;
  scroll-timeline-name: --container-timeline;
  scroll-timeline-axis: block; /* inline, x, y */
  /* we can also shorthand with `scroll-timeline: --container-timeline block;` */
}
```

- **block**(default): based on the language writing mode, for example, in English, it is vertical scroll
- **inline**: based on the inline direction, for example, in English, it is horizontal scroll
- **x**: horizontal scroll
- **y**: vertical scroll

## DO NOT put `animation-timeline` on top of the animation shorthand

Although `animation-timeline` cannot be set in the animation shorthand, the animation shorthand will set the value of `animation-timeline` to `auto` (default value, using document.timeline), so avoid placing `animation-timeline` before the animation shorthand.

```css
.square {
  width: 100px;
  height: 100px;
  background-color: gold;
  animation-timeline: --container-timeline;
  animation: linear(0, 0.25, 1) move; /* this line will set `animation-timeline` value to `auto` (default) */
  position: sticky;
  top: 0;
}
```

