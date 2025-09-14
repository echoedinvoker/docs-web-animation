# Animation Properties 1

```css
.square {
  width: 100px;
  height: 100px;
  background-color: gold;
  animation-name: move;
  animation-duration: 2s;
  animation-delay: 1s; /* same as transition-delay */
  animation-timing-function: ease-in-out; /* same as transition-timing-function */
  animation-iteration-count: 2; /* means the animation will run twice */
}                               /* you can also use 'infinite' to repeat indefinitely */
```

In the settings above, each animation will start from the original state. If we want the animation to start running back to the original state from the last ended state, we can use the `animation-direction` property.

```css
.square {
  width: 100px;
  height: 100px;
  background-color: gold;
  animation-name: move;
  animation-duration: 2s;
  animation-delay: 1s;
  animation-timing-function: ease-in-out;
  animation-iteration-count: 2;
  animation-direction: alternate; /* allows the animation to start from the end state */
}
```


The default value of `animation-direction` is `normal`, which means the animation will start from the original state each time. If set to `alternate`, the animation will run in reverse after each iteration. Other possible values include `reverse` (starting from the end state each time) and `alternate-reverse` (starting from the original state after running in reverse, but only after running in reverse).

Animations now start running when the page loads. If we want to prevent animations from running when the page loads, we can use the `animation-play-state` property.

```css
.square {
  width: 100px;
  height: 100px;
  background-color: gold;
  animation-name: move;
  animation-duration: 2s;
  animation-delay: 1s;
  animation-timing-function: ease-in-out;
  animation-iteration-count: 2;
  animation-direction: alternate;
  animation-play-state: paused; /* not running the animation at page load */
}                               /* you can use JS to change this to 'running' for starting the animation */
```

The above animation properties can be combined into a single `animation` property, which is a shorthand for all animation properties.

```css
.square {
  width: 100px;
  height: 100px;
  background-color: gold;
  animation: move 2s 1s ease-in-out 2 alternate paused; /* shorthand for all animation properties */
}
```

If we use `ease` as the name of an animation, we need to pay attention to the following issues.

```css
.square {
  width: 100px;
  height: 100px;
  background-color: gold;
  animation: ease 2s 1s ease-in-out 2 alternate paused;
  /*         ^^^^ will be treated as the timing function */
}
```

```css
.square {
  width: 100px;
  height: 100px;
  background-color: gold;
  animation: 2s 1s ease-in-out 2 alternate paused ease ;
  /*                                              ^^^^ will be treated as the animation name */
  /*                                                   because already have a timing function `ease-in-out` in front */
}
```

Therefore, try to put the animation name at the end of the shorthand property to avoid confusion. But the best practice is to avoid using any reserved words as animation names.



