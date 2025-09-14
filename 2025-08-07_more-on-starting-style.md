# more on starting style

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
        const button = document.querySelector('.toggle-button');
        button.addEventListener('click', function() {
          square.classList.toggle('blue');
        });
      })
    </script>
  </head>
  <body>
    <div class="square"></div>
    <button class="toggle-button">Toggle Square</button>
  </body>
</html>
```

```css
.square {
  width: 100px;
  height: 100px;
  background-color: yellow;
  transition: all 2s ease-in-out;
}

@starting-style {
  .square {
    background-color: skyblue;
  }
}

.square.blue {
  background-color: blue;
}
```

When the square loads, it will first display skyblue due to the starting-style, and then transition to yellow.

When the button is clicked for the first time, the square will transition from yellow to blue.

When the button is clicked again, the square will transition back to yellow. (*not skyblue, because the starting-style is only applied on load or display:none to something else*)




