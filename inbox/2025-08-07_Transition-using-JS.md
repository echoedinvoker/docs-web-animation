---
date: 2025-08-07
type: fact
aliases:
  -
hubs:
  - "[[web-animation]]"
---

# Transition using JS

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Transitions</title>
    <link href="styles.css" rel="stylesheet">
    <script>
      document.addEventListener("DOMContentLoaded", function() {
        const square = document.querySelector('.square');
        square.addEventListener('click', function(event) {
          console.log(event);
        });
      });
    </script>
  </head>
  <body>
    <div class="square"></div>
  </body>
</html>
```

```css
.square {
  width: 100px;
  height: 100px;
  background-color: purple;
}
```

```
PointerEvent {
  ...
  clientX: 129,  // The horizontal coordinate within the application's client area at which the event occurred
  clientY: 53, // The vertical coordinate within the application's client area at which the event occurred
  ...
}
```

application's client area is the area of the browser window where the web application is displayed, excluding any browser UI elements like toolbars or scrollbars.

We can use the `clientX` and `clientY` properties to set the transform style of the square based on the click position.

```html
    <script>
      document.addEventListener("DOMContentLoaded", function() {
        const square = document.querySelector('.square');
        document.addEventListener('click', function(event) {
          // Set the transform style of the square based on the click position
          square.style.transform = `translateY(${event.clientY}px) translateX(${event.clientX}px)`;
        });
      });
    </script>
```

Now, when you click anywhere in the document, the square will move to the position of the click. But the cursor position is at the top-left corner of the square, so we need to adjust the position to center based on the square's dimensions.

```html
    <script>
      document.addEventListener("DOMContentLoaded", function() {
        const square = document.querySelector('.square');
        document.addEventListener('click', function(event) {
          square.style.transform = `translateY(${
            event.clientY - square.clientHeight / 2
            //            ^^^^^^^^^^^^^^^^^^^^^^^^^ to center the square vertically
          }px) translateX(${
            event.clientX - square.clientWidth / 2
            //            ^^^^^^^^^^^^^^^^^^^^^^^^^ to center the square horizontally
          }px)`;
        });
      });
    </script>
```

Even we used JS to set the transform style, we can still use CSS transitions to animate the movement of the square. So we can add a transition to the square's CSS to make the movement smooth instead of instant.

```css
.square {
  width: 100px;
  height: 100px;
  background-color: purple;
  transition: transform 1s ease-in-out; /* Add transition for smooth movement */
}
```
