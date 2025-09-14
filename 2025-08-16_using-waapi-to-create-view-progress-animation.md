---
date: 2025-08-16
type: fact
aliases:
  -
hubs:
  - "[web-animation](./web-animation.md)"
---

# using waapi to create view progress animation

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
  border: 2px solid gold;
  box-sizing: border-box;
}

.content {
  padding: 30px;
  max-width: 700px;
  margin: 0 auto;
}

.progress {
  width: 100%;
  height: 100px;
  /* height: 150vh; */
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
}
```

```js
document.addEventListener("DOMContentLoaded", () => {});
```

## Create a view progress animation with WAAPI

```js
document.addEventListener("DOMContentLoaded", () => {
  const progressInner = document.querySelector(".progress-inner");

  const timeline = new ViewTimeline({
  //                   ^^^^^^^^^^^^
    subject: progressInner, // need to specify the subject element
    axis: "block"
  })

  progressInner.animate([
    { width: "0" },
    { width: "100%" }
  ], {
    fill: "both",
    timeline
  });
});
```


## Controlling the start and end ranges of the animation


```js
document.addEventListener("DOMContentLoaded", () => {
  const progressInner = document.querySelector(".progress-inner");

  const timeline = new ViewTimeline({
    subject: progressInner,
    axis: "block",
    inset: "auto 100px" // or you can use array the CSS.px func `["auto", CSS.px(100)]`
  })

  progressInner.animate([
    { width: "0" },
    { width: "100%" }
  ], {
    fill: "both",
    rangeStart: "cover 30%", // or you can use object options and CSS.percent func like:
                             // { rangeName: "cover", offset: CSS.percent(30) }
    timeline
  });
});
```

You can see that the `inset` and `range` options are defined in the different places, which is different from the CSS View Timeline syntax.


## Create another animation on the same ViewTimeline

We can use the same ViewTimeline object to create multiple animations and with WAAPI, we don't need to consider the scope issue like in CSS animations.


```js
document.addEventListener("DOMContentLoaded", () => {
  const progressInner = document.querySelector(".progress-inner");
  const container = document.querySelector(".container"); // we will animate the container background color

  const timeline = new ViewTimeline({
    subject: progressInner,
    axis: "block",
    inset: "auto 100px"
  })

  progressInner.animate([
    { width: "0" },
    { width: "100%" }
  ], {
    fill: "both",
    timeline
  });

  // create view progress animation for the container with the same ViewTimeline object
  container.animate([
    { backgroundColor: "salmon" },
  ], {
    fill: "both",
    timeline
  })
});
```


## Get some informations from the ViewTimeline object

```js
document.addEventListener("DOMContentLoaded", () => {
  const progressInner = document.querySelector(".progress-inner");
  const container = document.querySelector(".container");

  const timeline = new ViewTimeline({
    subject: progressInner,
    axis: "block",
    inset: "auto 100px"
  })

  console.log(timeline); // log the ViewTimeline object

  progressInner.animate([
    { width: "0" },
    { width: "100%" }
  ], {
    fill: "both",
    timeline
  });

  container.animate([
    { backgroundColor: "salmon" },
  ], {
    fill: "both",
    timeline
  })
});
```


```
ViewTimeline {
    axis: "block"
    currentTime: CSSUnitValue {value: -117.08108108108108, unit: 'percent'} // in view timeline, currentTime can be negative before the subject doesn't reach the startOffset yet.
    duration: CSSUnitValue {value: 100, unit: 'percent'}
    endOffset: CSSUnitValue {value: 2008, unit: 'px'} // this means how much scroll value is needed to reach the end of the animation
    source: div.container
    startOffset: CSSUnitValue {value: 1083, unit: 'px'} // this means how much scroll value is needed to reach the start of the animation
    subject: div.progress-inner
}

... scroll down to let subject on top of viewport

ViewTimeline {
    axis: "block"
    currentTime: CSSUnitValue {value: 172.64864864864865, unit: 'percent'} // in view timeline, currentTime can be over 100% when the subject exceeds the endOffset 
    duration: CSSUnitValue {value: 100, unit: 'percent'}
    endOffset: CSSUnitValue {value: 2008, unit: 'px'}
    source: div.container
    startOffset: CSSUnitValue {value: 1083, unit: 'px'}
    subject: div.progress-inner
}
```


