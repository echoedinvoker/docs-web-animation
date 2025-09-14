# Another Way to Define Keyframe Effect

## Define options in the scope of the keyframe

```js
document.addEventListener('DOMContentLoaded', function() {
  const element = document.querySelector('.square');
  const squareAnimation =element.animate([
    {
      transform: 'translateX(0px)',
      easing: 'ease-in', // like in CSS @keyframes, we can define options for each keyframe
    },                   // and it will override the global options
    {
      backgroundColor: 'blue',
      offset: 0.8,
      composite: 'replace', // like in CSS @keyframes, we can define options for each keyframe
    },
    {
      transform: 'translateX(calc(100vw - 100px)) rotate(360deg)',
      backgroundColor: 'crimson',
    }
  ], {
    duration: 3000,
    delay: 1000,
    direction: 'alternate',
    fill: 'both',
    iterations: Infinity,
    easing: 'linear', // this will be applied at keyframe 0%
    composite: 'add', // this will be applied at keyframe 80%
    timeline: document.timeline
  });
  // squareAnimation.pause();
});
```

## Another way to define keyframe effect

In addition to defining keyframe effects using the array of objects above, you can also use another way of defining them using objects.

```js
document.addEventListener('DOMContentLoaded', function() {
  const element = document.querySelector('.square');
  const squareAnimation =element.animate(
  {
    transform: [
      'translateX(0px)', // offset 0%
      'translateX(calc(100vw - 100px)) rotate(360deg)' // offset 100%
    ],
    backgroundColor: [
      'gold', // offset 0%
      'blue', // offset 50%
      'crimson' // offset 100%
    ],
  },
  {
    duration: 3000,
    delay: 1000,
    direction: 'alternate',
    fill: 'both',
    iterations: Infinity,
    easing: 'linear',
    composite: 'add',
    timeline: document.timeline
  });
  // squareAnimation.pause();
});
```

## Custom offsets

As mentioned above, the value of offset will be automatically determined based on the number of elements in the array, but we can also use a custom offset value, for example:

```js
document.addEventListener('DOMContentLoaded', function() {
  const element = document.querySelector('.square');
  const squareAnimation =element.animate(
  {
    transform: [
      'translateX(0px)', // offset 0%
      'translateX(calc(100vw - 100px)) rotate(360deg)' // offset 100%
    ],
    backgroundColor: [
      'gold', // offset 0%
      'blue', // offset 30% <-- custom offset -->
      'crimson' // offset 100%
    ],
    offset: [0, 0.3, 1] // replaces the default offsets of 0%, 50%, and 100% with custom offsets 0%, 30%, and 100%
  },
  {
    duration: 3000,
    delay: 1000,
    direction: 'alternate',
    fill: 'both',
    iterations: Infinity,
    easing: 'linear',
    composite: 'add',
    timeline: document.timeline
  });
  // squareAnimation.pause();
});
```

## Define options in the object of keyframe effect

In this way, we can also define options, but the values of the options will be defined in an *array format*, with each value in the array corresponding to the position in the offset array.

```js
document.addEventListener('DOMContentLoaded', function() {
  const element = document.querySelector('.square');
  const squareAnimation =element.animate(
  {
    transform: [
      'translateX(0px)',
      'translateX(calc(100vw - 100px)) rotate(360deg)'
    ],
    backgroundColor: [
      'gold',
      'blue',
      'crimson'
    ],
    offset: [0, 0.3, 1],
    // we can also define options but using ARRAY
    easing: ['ease-in', 'linear'],
    //          0%         30%      100% will repeat front value 'linear'
    composite: ['add', 'replace', 'add']
    //            0%      30%      100%
  },
  {
    duration: 3000,
    delay: 1000,
    direction: 'alternate',
    fill: 'both',
    iterations: Infinity,
    easing: 'linear',
    composite: 'add',
    timeline: document.timeline
  });
  // squareAnimation.pause();
});
```
