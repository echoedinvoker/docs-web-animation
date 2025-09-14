# anonymous scroll progress timeline

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
      <div class="square"></div>
      <div class="content">
        <div style="height: 200vh; background-color: black"></div>
      </div>
    </div>
    <div style="height: 200vh; background-color: #1a272f; margin: 20px"></div>
  </body>
</html>
```

```css
...
.container {
  height: 50vh;
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
  animation-timeline: /* there is no any named timeline... */
  position: sticky;
  top: 0;
}

@keyframes move {
  to {
    transform: translateX(calc(100vw - 100px));
  }
}
```

## Using an anonymous scroll progress timeline with `scroll()`

As shown above, in the absence of a named timeline, we want the `move` animation of `.square` to use the scroll progress timeline. We can use the `scroll()` function to implement an anonymous scroll progress timeline.


```css
.square {
  width: 100px;
  height: 100px;
  background-color: gold;
  animation: linear move;
  animation-timeline: scroll(block nearest); /* `scroll` is the function to use anonymous scroll progress */
  position: sticky;                          /* `block nearest` is the default value, eqaul to `scroll()` */
  top: 0;
}
```

You can give two values to `scroll()`, one is to determine the scrolling direction (block, inline, x, y), and the other is to use what as a reference to the scroll of a certain element as the scroll progress timeline.

In the example above, `nearest` represents using the nearest ancestral scroll container as the source of the scroll progress timeline, other options are:

- *root* - using the root scroll container as the source of the scroll progress timeline.
- *self* - using the scroll progress of the element itself as the source of the scroll progress timeline.

## Example for `scroll(self)`

```css
.container {
  height: 50vh;
  overflow: auto;
  background-color: #100e1e;
  animation: linear changeBg;
  animation-timeline: scroll(self);
}

@keyframes changeBg {
  to {
    background-color: coral;
  }
}
```

The example above changes the background color of `.container` when it changes its own scroll progress.

