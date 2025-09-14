# scroll progress range

```html
  <body>
    <div class="container">
      <div class="square"></div>
      <div class="content">
        <div style="height: 200vh; background-color: black"></div>
      </div>
    </div>
  </body>
```


```css
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
  animation: linear move;
  animation-timeline: scroll();
  position: sticky;
  top: 0;
}

@keyframes move {
  to {
    transform: translateX(calc(100vw - 100px));
  }
}
```

If we don't want the square to move all the time, but only when the user has scrolled a certain amount down the page, we can use the `animation-range-start` and `animation-range-end` properties to define a range of scroll progress where the animation should be active.

```css
.square {
  width: 100px;
  height: 100px;
  background-color: gold;
  animation: linear move;
  animation-timeline: scroll();

  /* The square will move only when the scroll position is between 20% and 80% of the scrollable area */
  /* This means it will start moving when the user has scrolled 20% down the page and stop moving when they reach 80% down the page */
  animation-range-start: 20%;
  animation-range-end: 80%;

  position: sticky;
  top: 0;
}
```

In the example above, you will find that when the scroll progress exceeds 80, the animation will return to its original position. We can use `animation-fill-mode: both` to make the animation stay in the last state even when it is outside the range.

```css
.square {
  width: 100px;
  height: 100px;
  background-color: gold;
  animation: linear move both;
  /*                     ^^^^ to ensure the animation applies both at the start and end of the range */
  animation-timeline: scroll();
  animation-range-start: 20%;
  animation-range-end: 80%;
  position: sticky;
  top: 0;
}
```

The scroll progress value does not necessarily have to be in percentage, it can also be in other units, such as pixel values:

```css
.square {
  width: 100px;
  height: 100px;
  background-color: gold;
  animation: linear move both;
  animation-timeline: scroll(block nearest);
  animation-range-start: 200px;
  /*                     ^^^^^ instead of a percentage, we can also use a pixel value */
  /*                           which means the animation will start when the scroll position is 200px down the page */
  animation-range-end: 80%;
  position: sticky;
  top: 0;
}
```

## Shorthand syntax `animation-range`


```css
.square {
  width: 100px;
  height: 100px;
  background-color: gold;
  animation: linear move both;
  animation-timeline: scroll(block nearest);
  animation-range: 200px 80%; /* we can use shorthand `animation-range` to combine both start and end values on one line */
  position: sticky;
  top: 0;
}
```

And we can also use the reversed keyword `normal` to indicate `0% 100%` range:


```css
.square {
  width: 100px;
  height: 100px;
  background-color: gold;
  animation: linear move both;
  animation-timeline: scroll(block nearest);
  animation-range: normal;
  /*               ^^^^^^ equal to `animation-range: 0% 100%` */
  position: sticky;
  top: 0;
}
```

When `normal` paired with a percentage in front of it, it means the range starts at that percentage and ends at 100% of the scrollable area:

```css
.square {
  width: 100px;
  height: 100px;
  background-color: gold;
  animation: linear move both;
  animation-timeline: scroll(block nearest);
  animation-range: 50% normal;
  /*               ^^^^^^^^^^ equal to `animation-range: 50% 100%` */
  position: sticky;
  top: 0;
}
```


When `normal` paired with a percentage in front of it, the `normal` keyword can be omitted, as it is the default value:

```css
.square {
  width: 100px;
  height: 100px;
  background-color: gold;
  animation: linear move both;
  animation-timeline: scroll(block nearest);
  animation-range: 50%;
  /*               ^^^ equal to `animation-range: 50% normal` */
  position: sticky;
  top: 0;
}
```

