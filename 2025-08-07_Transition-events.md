# Transition events

When we apply a transition to an element, the element will emit several events during the transition process. These events can be used to trigger additional actions or to monitor the progress of the transition.

```html
    <script>
      document.addEventListener("DOMContentLoaded", function() {
        const square = document.querySelector('.square');
        document.addEventListener('click', function(event) {
          square.style.transform = `translateY(${
            event.clientY - square.clientHeight / 2
          }px) translateX(${
            event.clientX - square.clientWidth / 2
          }px)`;
        });

        // Transition events
        square.addEventListener('transitionrun', function(event) {
        //                      ^^^^^^^^^^^^^^^ emits when a transition starts (before delay)
          console.log('Transition run:', event);
        });
        square.addEventListener('transitionstart', function(event) {
        //                      ^^^^^^^^^^^^^^^ emits when a transition starts (after delay)
          console.log('Transition started:', event);
        });
        square.addEventListener('transitionend', function(event) {
        //                      ^^^^^^^^^^^^^^^ emits when a transition ends
          console.log('Transition ended:', event);
        });
      });
    </script>
```

```css
.square {
  width: 100px;
  height: 100px;
  background-color: purple;
  transition: transform 2s 1s ease-in-out;
}
```

```
Transition run:
TtransitionEvent {
  ...
  elapsedTime: 0,
  propertyName: "transform",
  type: "transitionrun",
  ...
}

Transition started:
TtransitionEvent {
  ...
  elapsedTime: 0, // now we find this time is started after the delay
  propertyName: "transform",
  type: "transitionstart",
  ...
}

Transition ended:
TtransitionEvent {
  ...
  elapsedTime: 1, // because 2 - 1 = 1, which means duration includes delay, so the elapsed time is 1 second
  propertyName: "transform",
  type: "transitionend",
  ...
}
```

Let's use transition events to do something useful, like changing the color of the square after the transform transition ends.

```html
    <script>
      document.addEventListener("DOMContentLoaded", function() {
        const square = document.querySelector('.square');
        document.addEventListener('click', function(event) {
          square.style.transform = `translateY(${
            event.clientY - square.clientHeight / 2
          }px) translateX(${
            event.clientX - square.clientWidth / 2
          }px)`;
        });

        square.addEventListener('transitionend', function(event) {
          // give random color to the square after the transition ends
          square.style.backgroundColor = `rgb(${
            Math.floor(Math.random() * 256)
          }, ${
            Math.floor(Math.random() * 256)
          }, ${
            Math.floor(Math.random() * 256)}
          )`;
        });
      });
    </script>
```

Now, the color change is instantaneous, but we can also add a transition to the background color to make it smooth.

```css
.square {
  width: 100px;
  height: 100px;
  background-color: purple;
  transition: transform 1s ease-in-out,
    background-color 0.5s ease-in-out; /* transition for background color */
}
```

But now after the transform transition ends, the background color will change infinitely, because we also applied a transition to the background color, which will emit another `transitionend` event and lead to another color change. To prevent this, we can check the `propertyName` of the event to ensure we only change the color after the transform transition ends.

```html
    <script>
      document.addEventListener("DOMContentLoaded", function() {
        const square = document.querySelector('.square');
        document.addEventListener('click', function(event) {
          square.style.transform = `translateY(${
            event.clientY - square.clientHeight / 2
          }px) translateX(${
            event.clientX - square.clientWidth / 2
          }px)`;
        });

        square.addEventListener('transitionend', function(event) {
          if (event.propertyName !== 'transform') return; // only change color after transform transition ends
          square.style.backgroundColor = `rgb(${
            Math.floor(Math.random() * 256)
          }, ${
            Math.floor(Math.random() * 256)
          }, ${
            Math.floor(Math.random() * 256)}
          )`;
        });
      });
    </script>
```
