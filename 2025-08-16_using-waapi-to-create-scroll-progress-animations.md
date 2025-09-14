# using waapi to create scroll progress animations

```html
  <body>
    <div style="display: flex">
      <div class="container c-1">
        <div class="content">
          <div style="height: 200vh; background-color: #2d2337"></div>
          <div class="progress">
            <div class="progress-inner"></div>
          </div>
          <div style="height: 200vh; background-color: #2d2337"></div>
        </div>
      </div>
      <div class="container c-2">
        <div class="content">
          <div style="height: 200vh; background-color: #2d2337"></div>
          <div class="progress">
            <div class="progress-inner"></div>
          </div>
          <div style="height: 200vh; background-color: #2d2337"></div>
        </div>
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
  flex: 1;
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


## Create the scroll based animation

We can use WAAPI to create scroll based animations as follows:

```js
document.addEventListener("DOMContentLoaded", () => {
  const container1 = document.querySelector('.c-1'); // get the first container which has the scrollable content

  // Create a ScrollTimeline object from the container1
  const timeline = new ScrollTimeline({
    source: container1,
    axis: 'block'
  })

  // Create an animation on the container1 using the timeline we created
  container1.animate([
    { backgroundColor: 'salmon' },
  ], {
    fill: 'both',
    rangeStart: '30%',
    rangeEnd: '70%',
    timeline // use the timeline we created
  });
});
```


## Create the scroll based animation with a timeline which is out of the scope

In CSS, if the scope of the timeline source is different from the scope of the animation target, then `animation-scope` must be used to expand the timeline's scope. However, in Web Animations API (WAAPI), we do not need to do this, we can simply pass the timeline object directly.


```js
document.addEventListener("DOMContentLoaded", () => {
  const container1 = document.querySelector('.c-1');
  const container2 = document.querySelector('.c-2'); // Select the second container

  const timeline = new ScrollTimeline({
    source: container2, // instead of container1, we create the container1 animation based on container2 timeline
    axis: 'block'
  })

  container1.animate([
    { backgroundColor: 'salmon' },
  ], {
    fill: 'both',
    rangeStart: '30%',
    rangeEnd: '70%',
    timeline
  });
});
```

## Get some informations from the timeline object

```js
document.addEventListener("DOMContentLoaded", () => {
  const container1 = document.querySelector('.c-1');
  const container2 = document.querySelector('.c-2');

  const timeline = new ScrollTimeline({
    source: container2,
    axis: 'block'
  })

  console.log(timeline); // Log the timeline to see its properties

  container1.animate([
    { backgroundColor: 'salmon' },
  ], {
    fill: 'both',
    rangeStart: '30%',
    rangeEnd: '70%',
    timeline
  });
});
```


```
ScrollTimeline {
    axis: "block"
    currentTime: CSSUnitValue {value: 0, unit: 'percent'}
    //                         ^^^^^^^^^^^^^^^^^^^^^^^^^ this value changes as you scroll, and unit is percent
    duration: CSSUnitValue {value: 100, unit: 'percent'}
    //                      ^^^^^^^^^^^^^^^^^^^^^^^^^^^ this should be always 100%
    source: div.container.c-2
}
```

