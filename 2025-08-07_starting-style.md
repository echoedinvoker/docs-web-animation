# starting style

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
        const button = document.querySelector('.add-button');
        button.addEventListener('click', function() {
          const newSquare = square.cloneNode(true);
          document.querySelector('.wrapper').appendChild(newSquare);
        });
      })
    </script>
  </head>
  <body>
    <div class="wrapper">
      <div class="square"></div>
    </div>
    <button class="add-button">Add Square</button>
  </body>
</html>
```

```css
.square {
  width: 100px;
  height: 100px;
  background-image: linear-gradient(45deg, #ff0000, #0000ff);
  transition: all 2s allow-discrete;
}

.square.hide {
  display: none;
  opacity: 0;
}
```

In this example, you will find that the transition does not take effect when loading the page or when clicking the button to add elements.

This is because transitions by default do not occur in the following two cases:
- When an element changes from `display: none` to any visible state
- When an element is loaded

But we can use the `@starting-style` rule to define a starting style for the transition at the initial state of the element, which will allow the transition to occur when the element is added to the DOM or when it becomes visible.

```css
.square {
  width: 100px;
  height: 100px;
  background-image: linear-gradient(45deg, #ff0000, #0000ff);
  transition: all 2s allow-discrete;

  /* This is start style for the transition on the element initially or display from none to visible */
  @starting-style {
    opacity: 0;
  }
}

.square.hide {
  display: none;
  opacity: 0;
}
```

```css
.square {
  width: 100px;
  height: 100px;
  background-image: linear-gradient(45deg, #ff0000, #0000ff);
  transition: all 2s allow-discrete;
}
```

We can also extract the starting style into a separate rule, which can be useful for better organization or reusability.

```css
/* You can extract the starting style into a separate rule */
@starting-style {
  .square { /* because you extract it, you need to specify the selector again */
    opacity: 0;
  }
}

.square.hide {
  display: none;
  opacity: 0;
}
```


