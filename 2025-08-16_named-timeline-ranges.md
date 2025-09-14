---
date: 2025-08-16
type: fact
aliases:
  -
hubs:
  - "[web-animation](./web-animation.md)"
---

# named timeline ranges

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
  animation: animateWidth linear(0, 0.2, 1) both;
  animation-timeline: view();

  /* use values to define the start and end of the animation-range */
  animation-range-start: 10%;
  animation-range-end: 70%;
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

In the example above, numerical values are used to define `animation-range-start` and `animation-range-end`, we can also use some named range values as follows:

## `normal` and `cover`

`normal` and `cover` are the default behavior for `animation-range-start` and `animation-range-end`, respectively. They are equivalent to the values `0` and `100%`.

```css
.progress-inner {
  position: absolute;
  height: 100%;
  top: 0;
  left: 0;
  background-color: rgb(209, 90, 213);
  animation: animateWidth linear(0, 0.2, 1) both;
  animation-timeline: view();
  animation-range-start: normal; /* equivalent to 0 */
  animation-range-end: cover; /* equivalent to 100% */
}
```

## `contain`

The meaning of `contain` is that the subject should start or end the animation only when it is completely contained within the viewport.

```css
.progress-inner {
  position: absolute;
  height: 100%;
  top: 0;
  left: 0;
  background-color: rgb(209, 90, 213);
  animation: animateWidth linear(0, 0.2, 1) both;
  animation-timeline: view();
  animation-range-start: contain; /* start when the subject is just fully contained in the viewport */
  animation-range-end: contain 90%; /* end when the subject is just about to leave the viewport (still fully contained) and shifted (100% - 90%) down */
  /*                           ^^^ any named range value can be followed by a percentage value to further adjust the range */
}
```

![contain-animation-start.png](../assets/imgs/contain-animation-start.png)
![contain-animation-end.png](../assets/imgs/contain-animation-end.png)


## `contain` with the subject which is higher than the viewport

When the subject is higher than the viewport, `contain` will start the animation when the subject is fully filled in the viewport, and end the animation when the subject is about to not be fully filled in the viewport anymore.

```css
.progress {
  width: 100%;
  height: 150vh; /* set the height of the subject to be higher than the viewport */
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
  animation: animateWidth linear(0, 0.2, 1) both;
  animation-timeline: view();
  animation-range-start: contain;
  animation-range-end: contain;
}
```

![higher-sub-start.png](../assets/imgs/higher-sub-start.png)

![higher-sub-end.png](../assets/imgs/higher-sub-end.png)

## `entry`

`entry` is defined by the logic of when the subject has just entered or fully entered the viewport to determine the animation scope.

```css
.progress-inner {
  position: absolute;
  height: 100%;
  top: 0;
  left: 0;
  background-color: rgb(209, 90, 213);
  animation: animateWidth linear(0, 0.2, 1) both;
  animation-timeline: view();
  animation-range-start: entry;
  animation-range-end: entry;
}
```


![entry-animation-start.png](../assets/imgs/entry-animation-start.png)

![entry-animation-end.png](../assets/imgs/entry-animation-end.png)


![higher-sub-entry-start.png](../assets/imgs/higher-sub-entry-start.png)

When the subject is higher than the viewport, many people may misunderstand the animation end state when using `entry`, please refer to the following figure.

![higher-sub-entry-end.png](../assets/imgs/higher-sub-entry-end.png)

## `exit`

`exit` is defined by the logic of when the subject has just exited or fully exited the viewport to determine the animation scope.


```css
.progress-inner {
  position: absolute;
  height: 100%;
  top: 0;
  left: 0;
  background-color: rgb(209, 90, 213);
  animation: animateWidth linear(0, 0.2, 1) both;
  animation-timeline: view();
  animation-range-start: exit;
  animation-range-end: exit;
}
```


![exit-animation-start.png](../assets/imgs/exit-animation-start.png)



![exit-animation-end.png](../assets/imgs/exit-animation-end.png)



![higher-sub-exit-start.png](../assets/imgs/higher-sub-exit-start.png)


![higher-sub-exit-end.png](../assets/imgs/higher-sub-exit-end.png)

## `entry-crossing`

`entry-crossing` is similar to `entry`, with no difference in smaller subjects, only when the subject is higher than the viewport and the animation ends.


```css
.progress-inner {
  position: absolute;
  height: 100%;
  top: 0;
  left: 0;
  background-color: rgb(209, 90, 213);
  animation: animateWidth linear(0, 0.2, 1) both;
  animation-timeline: view();
  animation-range-start: entry-crossing;
  animation-range-end: entry-crossing;
}
```


![higher-sub-entry-crossing-end.png](../assets/imgs/higher-sub-entry-crossing-end.png)


## `exit-crossing`

`exit-crossing` is similar to `exit`, with no difference in smaller subjects, only when the subject is higher than the viewport and the animation starts.

```css
.progress-inner {
  position: absolute;
  height: 100%;
  top: 0;
  left: 0;
  background-color: rgb(209, 90, 213);
  animation: animateWidth linear(0, 0.2, 1) both;
  animation-timeline: view();
  animation-range-start: exit-crossing;
  animation-range-end: exit-crossing;
}
```


![higher-sub-exit-crossing-start.png](../assets/imgs/higher-sub-exit-crossing-start.png)

## Tool 

You can use the following tool to visualize these ranges:

https://scroll-driven-animations.style/tools/view-timeline/ranges/#range-start-name=cover&range-start-percentage=0&range-end-name=cover&range-end-percentage=100&view-timeline-axis=block&view-timeline-inset=0&subject-size=smaller&subject-animation=reveal&interactivity=clicktodrag&show-areas=yes&show-fromto=yes&show-labels=yes





