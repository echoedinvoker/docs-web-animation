# discrete animation

## Discrete Animation

Similar to discrete animation with transitions, animations can also use discrete animation for certain properties.

The only difference is that you do not need to use `allow-discrete`, the animation will automatically determine whether the property needs to use discrete animation.

```css
.square {
  width: 100px;
  height: 100px;
  background-image: linear-gradient(gold, purple); /* this property cannot be computed values but it can be animated discretely */
  animation: bg-flip 10s;
  /* don't need to `allow-discrete` like transitions */
}

@keyframes bg-flip {
  100% {
    background-image: linear-gradient(greenyellow, maroon); /* it'll flip to this background image at the midway of the animation (0s + 10s * 1) / 2 = 5s */
  }
}
```

But after the animation ends, the background image will flip back to the original one, which is not what we want.

We can use `animation-fill-mode: forwards` to prevent the background image from flipping back to the original one.

```css
.square {
  width: 100px;
  height: 100px;
  background-image: linear-gradient(gold, purple);
  animation: bg-flip 10s;
  animation-fill-mode: forwards; /* to aviod the background image fliping back to original when animation ends */
}

@keyframes bg-flip {
  100% {
    background-image: linear-gradient(greenyellow, maroon);
  }
}
```

Here is a more complex example that uses the discrete animation feature to animate the background image with more keyframes.

```css
.square {
  width: 100px;
  height: 100px;
  background-image: linear-gradient(gold, purple);
  animation: bg-flip 10s;
  animation-fill-mode: forwards;
}

@keyframes bg-flip {
  50% { /* (0s + 10s * 0.5) / 2 = 2.5s */
    background-image: linear-gradient(violet, skyblue);
  }
  100% { /* (2.5s + 10s * 1 ) / 2 = 6.25s */
    background-image: linear-gradient(greenyellow, maroon);
  }
}
```

Knowing how to calculate the time of when the background image will flip is important.

## Discrete Animation with `display` Property

Similar to transition discrete animation, when animation involves the `display` property, it will switch to `display: none` at the end of the animation instead of midway through.

```css
.square {
  width: 100px;
  height: 100px;
  background-image: linear-gradient(gold, purple);
  animation: bg-flip 10s, fade-out 10s;
  /*                      ^^^^^^^^^^^^ */
  animation-fill-mode: forwards;
}

@keyframes bg-flip {
  50% {
    background-image: linear-gradient(violet, skyblue);
  }
  100% {
    background-image: linear-gradient(greenyellow, maroon);
  }
}

/* add this animation to flip the `display` property */
@keyframes fade-out {
  0% {
    display: block;
  }
  100% {
    display: none;
  }
}
```

This feature allows for easy switching of display and animation with other properties, such as opacity.

```css
.square {
  width: 100px;
  height: 100px;
  background-image: linear-gradient(gold, purple);
  animation: bg-flip 10s, fade-out 10s;
  animation-fill-mode: forwards;
}

@keyframes bg-flip {
  50% {
    background-image: linear-gradient(violet, skyblue);
  }
  100% {
    background-image: linear-gradient(greenyellow, maroon);
  }
}

@keyframes fade-out {
  0% {
    display: block;
    opacity: 1; /* display animation feature make it easy to pair with other properties */
  }
  100% {
    display: none;
    opacity: 0;
  }
}
```

When the animation switches from `display: none` to `display: block` (or any other visible display), it will switch at the beginning of the animation, rather than at the end of the animation, in order to facilitate use with other properties.

```css
.square {
  width: 100px;
  height: 100px;
  background-image: linear-gradient(gold, purple);
  animation: bg-flip 10s, fade-in 10s;
  /*                      ^^^^^^^ change fade-out to fade-in */
  animation-fill-mode: forwards;
}

@keyframes bg-flip {
  50% {
    background-image: linear-gradient(violet, skyblue);
  }
  100% {
    background-image: linear-gradient(greenyellow, maroon);
  }
}

/* change fade-out to fade-in */
@keyframes fade-in {
  0% {
    display: none;
    opacity: 0;
  }
  100% {
    display: block; /* it'll flip to block when animation starts instead of 5s */
    opacity: 1;
  }
}
```
