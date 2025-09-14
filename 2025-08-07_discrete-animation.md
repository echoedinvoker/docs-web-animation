# discrete animation

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Transitions</title>
    <link href="styles.css" rel="stylesheet">
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
  background-image: linear-gradient(purple, yellow);
  transition: all 2s;
}

.square:hover {
  background-image: linear-gradient(yellow, purple);
}
```

Even though we use `all` in the `transition` property above, the change in background-image still switches instantly, as if no transition is used at all. This is because we only allow transitions using computed values by default, and background-image cannot apply computed value transitions. Therefore, the browser will ignore this transition.

The meaning of computed values is that the browser will calculate the value that each time attribute should have between the start and end state, and then interpolate it between the start and end state.

However, background-image can use *discrete transitions*, meaning the property will switch to the end state in the middle of the duration (in this case, 1s), without needing any computed value interpolation. We can activate it using the following syntax.

```css
.square {
  width: 100px;
  height: 100px;
  background-image: linear-gradient(purple, yellow);
  transition: all 2s;
  transition-behavior: allow-discrete; /* This line allows discrete transitions */
}

.square:hover {
  background-image: linear-gradient(yellow, purple);
}
```

Now when we hover over the square, the background-image will change to the end state after 1 second, without any intermediate steps.

We can also use the `allow-discrete` keyword in the shorthand `transition` property.

```css
.square {
  width: 100px;
  height: 100px;
  background-image: linear-gradient(purple, yellow);
  transition: all 2s allow-discrete;
  /*                 ^^^^^^^^^^^^^^ it can also be used in the shorthand */
}

.square:hover {
  background-image: linear-gradient(yellow, purple);
}
```

We can use it together with other computed value transitions. Below, we add the property `transform` and apply a transition to it. Even if we use `allow-discrete`, this property will still use a regular transition because it is a computed value transition.


```css
.square {
  width: 100px;
  height: 100px;
  background-image: linear-gradient(purple, yellow);
  transition: all 2s allow-discrete;
}

.square:hover {
  background-image: linear-gradient(yellow, purple);
  transform: translateY(100px); /* add a new end state property to move the square down */
}                               /* it will use regular transition by default */
```

You can see the square moving down smoothly (using a computed value transition) while the background-image changes instantly after 1 second (using a discrete transition).

You can see all kinds of transitions in the below link:
[Animation types](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_animated_properties)

You can check which properties support what kind of transitions in the following link:
[Animation types for properties](https://vallek.github.io/animatable-css/)



