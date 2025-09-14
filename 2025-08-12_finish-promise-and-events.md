# finish promise and events

Whenever we play an animation, a promise `finished` will be created for the animation object. This promise will resolve when the animation is finished.

We can wait for this promise to resolve using `await` keyword (or then), and then do something after the animation is finished.

```js
document.addEventListener("DOMContentLoaded", async () => {
  //                                          ^^^^^ for await `finished` promise from the animation object
  const element = document.querySelector(".square");
  const squareAnimation = element.animate( ... );

  const buttons = document.querySelectorAll(".button");
  buttons.forEach((button) => { ... });

  // ...

  squareAnimation.pause();

  await squareAnimation.finished; // whenever playing animation, a promise `finished` will be created
                                  // and it will resolve when the animation is finished
  element.remove()  // so we can do something after the animation is finished by awaiting the promise
  console.log("Animation finished and element removed from DOM.");
});
```

Animation object also provides an event `finish` that is fired when the animation is finished. This event can be used to do the same thing as the `finished` promise.

```js
document.addEventListener("DOMContentLoaded", () => {
  const element = document.querySelector(".square");
  const squareAnimation = element.animate( ... );

  const buttons = document.querySelectorAll(".button");
  buttons.forEach((button) => { ... });

  // ...

  squareAnimation.pause();

  squareAnimation.addEventListener("finish", (e) => {
  //                                          ^ for `finish` event from the animation object
    element.remove();
    console.log(e)
  })
});
```

```
AnimationPlaybackEvent {
  ...
  currentTarget: Animation { <-- animation object
    ...
    effect: KeyframeEffect { <-- animation effect object
      ...
    }
  }
}
```


Similar to the `finish` event, there is also a `cancel` event that is fired when the animation is cancelled. This can happen if we call `cancel()` method on the animation object or if the animation is removed from the DOM before it finishes.

```js
document.addEventListener("DOMContentLoaded", () => {
  const element = document.querySelector(".square");
  const squareAnimation = element.animate( ... );

  const buttons = document.querySelectorAll(".button");
  buttons.forEach((button) => { ... });

  // ...

  squareAnimation.pause();

  squareAnimation.addEventListener("cancel", (e) => {
  //                                ^^^^^^^   ^  similar to `finish` event, but for when the animation is cancelled
    element.remove();
    console.log(e)
  })
});
```

evnet object for `cancel` is similar to `finish` event:

```
AnimationPlaybackEvent {
  ...
  currentTarget: Animation { <-- animation object
    ...
    effect: KeyframeEffect { <-- animation effect object
      ...
    }
  }
}
```
