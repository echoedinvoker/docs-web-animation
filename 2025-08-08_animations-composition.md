# animations composition

## Issue with multiple animations

```css
.square {
  width: 100px;
  height: 100px;
  background-color: gold;
  animation: move 2s ease-in-out 2 alternate, bounce 0.3s ease-in-out alternate infinite;
}

@keyframes move {
  0% {
    transform: translateX(0);
  }
  100% {
    transform: translateX(calc(100vw - 140px));
  }
}

@keyframes bounce {
  0% {
    transform: translateY(0);
  }
  100% {
    transform: translateY(100px);
  }
}
```

Above example will not work as expected, because the `move` and `bounce` use the same `transform` property, so the last one will override the first one.

## Add animation-composition

```css
.square {
  width: 100px;
  height: 100px;
  background-color: gold;
  animation: move 2s ease-in-out 2 alternate, bounce 0.3s ease-in-out alternate infinite;
  animation-composition: add; /* This property will combine the animations */
  /* 0% { */
  /*   transform: translateX(0) translateY(0); */
  /* } */
  /* 100% { */
  /*   transform: translateX(calc(100vw - 140px)) translateY(100px); */
  /* } */
}
```

Default value for `animation-composition` is `replace`, which means the last animation will override the previous ones. If you want to combine the animations, you can use `add` or `acc` (accumulate).

```css
.square {
  width: 100px;
  height: 100px;
  background-color: gold;
  transform: rotate(45deg); /* Initial transform */
  animation: move 2s ease-in-out 2 alternate;
  animation-composition: add; /* this will also apply to the initial transform */
}

@keyframes move {
  0% {
    transform: translateX(0); /* combines with initial transform */
    /* becomes rotate(45deg) translateX(0), the order matters because rotate will effect the translationX direction */
  }
  100% {
    transform: translateX(500px);
    /* becomes rotate(45deg) translateX(500px) */
  }
}
```


```css
.square {
  width: 100px;
  height: 100px;
  background-color: gold;
  transform: rotate(45deg);
  animation: move 2s ease-in-out 2 alternate;
  animation-composition: add;
}

@keyframes move {
  0% {
    transform: translateX(0) rotate(45deg);
    /*                       ^^^^^^^^^^^^^ this rotate is after the translateX, so it won't effect the translation direction */
    /* with animation-composition:add -> rotate(45deg) translateX(0) rotate(45deg) */
  }
  100% {
    transform: translateX(500px);
  }
}
```

So, if rotate is applied before translateX, it will affect the translation direction. If placed after, it will not affect the translation direction.

## Acc animation-composition

```css
.square {
  width: 100px;
  height: 100px;
  background-color: gold;
  transform: rotate(45deg);
  animation: move 2s ease-in-out 2 alternate;
  animation-composition: acc;
  /*                     ^^^ similar to add, but all same transforms are accumulated to the last one */
}

@keyframes move {
  0% {
    transform: translateX(0) rotate(45deg);
    /* with animation-composition:acc -> translateX(0) rotate(90deg) */
    /* so translateX won't be affected by the rotate */
  }
  100% {
    transform: translateX(500px);
  }
}
