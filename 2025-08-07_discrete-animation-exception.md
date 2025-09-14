---
date: 2025-08-07
type: fact
aliases:
  -
hubs:
  - "[web-animation](./web-animation.md)"
---

# discrete animation exception

Usually, discrete animation switches to the end state in the middle of the duration, except for the display property.

If using discrete animation to switch the display property:
  - any visable value -> none: the value switches to none at the end of the duration
  - none -> any visible value: the value switches to the visible value at the start of the duration

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    ...
    <script>
      document.addEventListener("DOMContentLoaded", function() {
        const square = document.querySelector('.square');
        const button = document.querySelector('.toggle-button');
        button.addEventListener('click', function() {
          square.classList.toggle('hide');
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
  background-image: linear-gradient(45deg, #ff0000, #0000ff);
  transition: display 2s allow-discrete;
}

.square.hide {
  display: none;
}
```

Therefore, in the example above, when the button is clicked in the square where it is visible, it will disappear after 2 seconds (end of duration). When the button is clicked in the square where it is not visible, it will immediately appear (start of duration).

The purpose of this design is to facilitate the use of other attributes that can use computed value transition, such as opacity, transform, and so on.

```css
.square {
  width: 100px;
  height: 100px;
  background-image: linear-gradient(45deg, #ff0000, #0000ff);
  transition: all 2s allow-discrete;
  /*          ^^^ use `all` to apply transition to `opacity` also */
}

.square.hide {
  display: none;
  opacity: 0; /* this will be applied computed value transition */
}
```

Above, we added an opacity transition so that when the square is displayed, clicking the button will cause the square to fade out for 2 seconds at first (regular transition), and then hide with display:none (discrete animation).





