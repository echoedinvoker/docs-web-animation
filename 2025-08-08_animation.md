---
date: 2025-08-08
type: fact
aliases:
  -
hubs:
  - "[web-animation](./web-animation.md)"
---

# animation

In transition, we can only animate from one state to another. We do not have control over the intermediate states. And transition need to be triggered by an event, such as a click or a hover. But sometimes we want to animate without any user interaction, such as a loading spinner or a progress bar. In this case, we can use the `animations`.

`animations` don't need to be triggered by an event, they have full control over the intermediate states, and they don't require writing any JavaScript code.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="stylesheet" href="styles.css" />
    <title>Animations</title>
    <script>
      document.addEventListener("DOMContentLoaded", () => {});
    </script>
  </head>
  <body>
    <div class="square"></div>
  </body>
</html>
```

```css
body {
  background-color: #1d1d1d;
  font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
    Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
  padding: 20px;
}

.square {
  width: 100px;
  height: 100px;
  background-color: gold;
}
```

Above is a simple HTML and CSS setup with a square. Now, let's add an animation to the square.

First, we need to define the animation using the `@keyframes` rule.
```css

@keyframes move {
  from { /* `from` is equivalent to 0% out of time */
    transform: translateX(0);
  }
  to { /* `to` is equivalent to 100% out of time */
    transform: translateX(calc(100vw - 140px));
  }
}
```

Then, we apply the animation to the square by adding the `animation-name` and `animation-duration` properties to the `.square` class.

```css
.square {
  width: 100px;
  height: 100px;
  background-color: gold;
  animation-name: move; /* The name of the animation defined above */
  animation-duration: 2s; /* The duration of the animation */
}

@keyframes move {
  from {
    transform: translateX(0);
  }
  to {
    transform: translateX(calc(100vw - 140px));
  }
}
```

Now, when you open the HTML file in a browser, the square will move from the left side of the screen to the right side over a duration of 2 seconds *without any user interaction*.

But it's all switch from start state to end state, what different from transition?
The key difference is that `animations` allow you to define multiple intermediate states, not just the start and end states. This is done using percentages in the `@keyframes` rule.

```css
@keyframes move {
  0% {
    transform: translateX(0);
  }
  50% { /* we can add intermediate states as much as we want */
    transform: translateX(80px); /* intermediate state is defined by ourself, not by browser, this is the key difference from transition */
  }
  100% {
    transform: translateX(calc(100vw - 140px));
  }
}
```
```
