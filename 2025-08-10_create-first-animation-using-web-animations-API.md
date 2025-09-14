# Create First Animation Using Web Animations API

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="stylesheet" href="styles.css" />
    <title>Web Animations API</title>
    <script src="./script.js"></script>
  </head>
  <body>
    <div class="square"></div>
  </body>
</html>
```

```js
// script.js
document.addEventListener('DOMContentLoaded', function() {
  const element = document.querySelector('.square');
});
```

```css
/* styles.css */
.square {
  width: 100px;
  height: 100px;
  background-color: gold;
}
```

We can use Web Animations API to create animations in JavaScript:

```js
document.addEventListener('DOMContentLoaded', function() {
  const element = document.querySelector('.square');

  // create an Animation effect with KeyframeEffect (in the future we can use other types of effects)
  const squareAnimationKeyframes = new KeyframeEffect(element, [
    { transform: 'translateX(0px)' },
    {
      backgroundColor: 'blue',
      offset: 0.8, // by default is 0.5, if you want to change it, need to use `offset` property
    },
    {
      transform: 'translateX(calc(100vw - 100px)) rotate(360deg)',
      backgroundColor: 'crimson',
    }
  ],
  3000 // duration in milliseconds, must be a positive number instead of a string in CSS
  );

  // create an Animation object with animation effect and timeline
  const squareAnimation = new Animation(
    squareAnimationKeyframes,
    document.timeline // document's timeline is default value, can be omitted
  );                  // we can rely on other type of timelines such as scroll timeline

  console.log(squareAnimationKeyframes, squareAnimation); // let's check the console to see the structure of KeyframeEffect and Animation objects
});
```


```
KeyframeEffect {
    composite: "replace"
    pseudoElement: null
    target: div.square
    [Prototype](./Prototype.md): KeyframeEffect {  // in the prototype we can find methods to manipulate keyframes
        getKeyframes: ƒ getKeyframes()
        setKeyframes: ƒ setKeyframes()
        ...
    }
}

Animation {
    ...
    effect: KeyframeEffect { ... }  // the animation effect will be put into `effect` property
    timeline: DocumentTimeline {currentTime: 134906.368, duration: null}
    [Prototype](./Prototype.md): Animation { // in the prototype we can find methods to manipulate the animation
        cancel: ƒ cancel()
        finish: ƒ finish()
        pause: ƒ pause()
        persist: ƒ persist()
        play: ƒ play()
        reverse: ƒ reverse()
        ...
    }
}
```

So, we can use Animation object to control the animation, such as play, pause, cancel, etc.

```js
document.addEventListener('DOMContentLoaded', function() {
  const element = document.querySelector('.square');
  const squareAnimationKeyframes = new KeyframeEffect(element, [
    { transform: 'translateX(0px)' },
    {
      backgroundColor: 'blue',
      offset: 0.8,
    },
    {
      transform: 'translateX(calc(100vw - 100px)) rotate(360deg)',
      backgroundColor: 'crimson',
    }
  ], 3000);
  const squareAnimation = new Animation(
    squareAnimationKeyframes,
    document.timeline
  );
  squareAnimation.play(); // Start the animation
});
```

You can see the square moving from left to right, changing its color from gold to blue and then to crimson.

But if we want the animation run infinitely, we can replace the `duration` parameter with an object, which allows us to specify more options such as `delay`, `direction`, `fill`, `iterations`, `easing`, and `composite`:

```js
document.addEventListener('DOMContentLoaded', function() {
  const element = document.querySelector('.square');
  const squareAnimationKeyframes = new KeyframeEffect(element, [
    { transform: 'translateX(0px)' },
    {
      backgroundColor: 'blue',
      offset: 0.8,
    },
    {
      transform: 'translateX(calc(100vw - 100px)) rotate(360deg)',
      backgroundColor: 'crimson',
    }
  ],
    // instead of simply passing duration, we can pass an object with more options
    {
      duration: 3000,
      delay: 1000,
      direction: 'alternate',
      fill: 'both',
      iterations: Infinity,
      easing: 'linear',
      composite: 'add',
    }
  );
  const squareAnimation = new Animation(
    squareAnimationKeyframes,
    document.timeline
  );
  squareAnimation.play();
});
```

Typically, we do not use the above method to create an Animation object, instead we directly use the `element.animate()` method to quickly create animations and automatically play them.

```js
document.addEventListener('DOMContentLoaded', function() {
  const element = document.querySelector('.square');

  // instead of creating an Animation effect and then an Animation object then playing it,
  // we can directly use the element.animate() method to fast create an animation and play it automatically
  element.animate([
    { transform: 'translateX(0px)' },
    {
      backgroundColor: 'blue',
      offset: 0.8,
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
    easing: 'linear',
    composite: 'add',
    timeline: document.timeline // add a `timeline` property to specify the timeline, can be omitted if using document's timeline
  });
  // this way, the animation will be played automatically, so we don't need to call `play()` method
});
```

`element.animate()` method returns an Animation object, so we can use it to control the animation, such as pause, cancel, etc. If we don't want the animation to start automatically, we can pause it right after creating it:

```js
document.addEventListener('DOMContentLoaded', function() {
  const element = document.querySelector('.square');
  const squareAnimation = element.animate([
//^^^^^^^^^^^^^^^^^^^^^^^ this method returns an Animation object
    { transform: 'translateX(0px)' },
    {
      backgroundColor: 'blue',
      offset: 0.8,
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
    easing: 'linear',
    composite: 'add',
    timeline: document.timeline
  });
  squareAnimation.pause(); // we can use the Animation object to control the animation
                           // in this case, we pause the animation, so it won't start automatically
});
```
