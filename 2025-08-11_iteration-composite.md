# Iteration Composite

This is a feature of the Web Animations API that allows you to accumulate the effects of each iteration of an animation. It is not available in CSS animations, only in the Web Animations API.

```js
document.addEventListener('DOMContentLoaded', function() {
  const element = document.querySelector('.square');
  const squareAnimation =element.animate(
  {
    transform: [
      'translateX(0px)',
      'translateX(calc(100vw - 100px)) rotate(360deg)'
    ],
  },
  {
    duration: 3000,
    iterations: Infinity,
    iterationComposite: 'accumulate', // this option is only exist in Web Animations API, not in CSS
  });
  // squareAnimation.pause();
});
```

Let's simulate each iteration of the animation:


```js
document.addEventListener('DOMContentLoaded', function() {
  const element = document.querySelector('.square');
  const squareAnimation =element.animate(
  {
    transform: [
      'translateX(0px)',
      // 2nd iteration will be `translateX(calc(0 + 100vw - 100px)) rotate(360deg)`
      // 3rd iteration will be `translateX(calc(0 + (100vw - 100px) * 2)) rotate(calc(360deg * 2))`

      'translateX(calc(100vw - 100px)) rotate(360deg)'
      // 2nd iteration will be `translateX(calc(0 + (100vw - 100px) * 2)) rotate(calc(360deg * 2))`
      // 3rd iteration will be `translateX(calc(0 + (100vw - 100px) * 3)) rotate(calc(360deg * 3))`
    ],
  },
  {
    duration: 3000,
    iterations: Infinity,
    iterationComposite: 'accumulate',
  });
  // squareAnimation.pause();
});
```

You can see that 0% properties' values are accumulated with the last 100% properties' values of the previous iteration. This is useful for creating animations that build upon themselves, such as a bouncing ball that gets higher with each bounce or a spinning object that spins faster with each iteration.

> Be NOTE! this feature only works on the FIREFOX browser, not on the CHROME browser right now.



