# pending state

## pending state and ready promise

Actions of animation objects has a pending state, which means that the action is not yet complete and it's not ready for the next action.

When the pending state is true, the property `ready` of the animation object returns a promise, when that promise resolves, the animation is ready for the next action and the pending state is false.

```js
document.addEventListener("DOMContentLoaded", () => {
  const element = document.querySelector(".square");
  const squareAnimation = element.animate( ...);

  const buttons = document.querySelectorAll(".button");
  buttons.forEach((button) => { ... });

  // ...

  squareAnimation.pause();

  console.log("playState after pause:", squareAnimation.playState);
  console.log("pending after pause:", squareAnimation.pending);

  squareAnimation.ready.then(() => {
    console.log("Animation is ready");
    console.log("playState after ready:", squareAnimation.playState);
    console.log("pending after ready:", squareAnimation.pending);
  });
});
```

```
playState after pause: paused
pending after pause: true
Animation is ready
playState after ready: paused
pending after ready: false
```

Above example shows that after calling `pause()`, the pending state is true, and the `ready` promise resolves when the animation is ready for the next action.

## A tricky example

```js
  // ...

  squareAnimation.pause();
  console.log("playState after pause:", squareAnimation.playState);
  console.log("pending after pause:", squareAnimation.pending);

  squareAnimation.ready.then(() => {
    console.log("Animation is ready");
    console.log("playState after ready:", squareAnimation.playState);
    console.log("pending after ready:", squareAnimation.pending);
  });

  // here we execute another action `play()` 
  squareAnimation.play();
  console.log("playState after play:", squareAnimation.playState);
  console.log("pending after play:", squareAnimation.pending);
```

What happens here is that the `play()` method is called while the animation is still in the pending state after the `pause()`.

```
playState after pause: paused
pending after pause: true
playState after play: running
pending after play: true
Animation is ready
playState after ready: running
pending after ready: false
```

You can see that the `play()` method is called while the animation is still pending, and it will be executed after the pause is complete, but the ready promise won't resolve until all the actions are complete and the pending state switches to false after the `ready` promise resolves.

So you can see the `playState after ready` is on the bottom of the output.

## Check other actions

There are several actions that can be performed on the animation object, let's see how they affect the pending state and play state.

### play()

```js
document.addEventListener("DOMContentLoaded", () => {
  const element = document.querySelector(".square");
  const squareAnimation = element.animate( ... );

  const buttons = document.querySelectorAll(".button");
  buttons.forEach((button) => {
    button.addEventListener("click", () => {
      if (button.classList.contains("play")) {
        squareAnimation.play();
        console.log("playState after play:", squareAnimation.playState);
        console.log("pending after play:", squareAnimation.pending);
      }
    //...
  });

  //...

  squareAnimation.pause();
});
```

```
playState after play: running
pending after play: true
```


### cancel()

```js
document.addEventListener("DOMContentLoaded", () => {
  const element = document.querySelector(".square");
  const squareAnimation = element.animate( ... );

  const buttons = document.querySelectorAll(".button");
  buttons.forEach((button) => {
    button.addEventListener("click", () => {
      //...
      if (button.classList.contains("cancel")) {
        squareAnimation.cancel();
        console.log("playState after cancel:", squareAnimation.playState);
        console.log("pending after cancel:", squareAnimation.pending);
      }
    //...
  });

  //...

  squareAnimation.pause();
});
```

```
playState after cancel: idle
pending after cancel: false <-- no pending state after cancel, it can execute the next action immediately
```

### finish()

```js
document.addEventListener("DOMContentLoaded", () => {
  const element = document.querySelector(".square");
  const squareAnimation = element.animate( ... );

  const buttons = document.querySelectorAll(".button");
  buttons.forEach((button) => {
    button.addEventListener("click", () => {
      //...
      if (button.classList.contains("finish")) {
        squareAnimation.finish();
        console.log("playState after finish:", squareAnimation.playState);
        console.log("pending after finish:", squareAnimation.pending);
      }
    //...
  });

  //...

  squareAnimation.pause();
});
```

```
playState after finish: finished
pending after finish: false <-- no pending state after finish, it can execute the next action immediately
```
