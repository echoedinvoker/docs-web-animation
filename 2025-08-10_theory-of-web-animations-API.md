---
date: 2025-08-10
type: fact
aliases:
  -
hubs:
  - "[web-animation](./web-animation.md)"
---

# theory of web animations API

Web Animations API has two models:

## Timing Model

Whenever we open any window in our browser, by default we have an object called the `timeline object`.

`timeline object` is a global object that starts whenever we open a window, and it keeps track of the time until the window is closed.

So it relys on the window being open and the time being tracked, and our all animations are based on it to calculate the time. (In more advanced courses, we will learn how to make the `timeline object` depend on other things, such as scroll position.)

We can directly access the `timeline object` by keying `document.timeline` in the console.

```
document.timeline
DocumentTimeline {
  currentTime: 24261.922, <--------- elapsed time in milliseconds since the window was opened
  duration: null,
  [Prototype](./Prototype.md): DocumentTimeline
}
```

![all-animations-rely-on-timeline.png](../assets/imgs/all-animations-rely-on-timeline.png)
When you are creating animations, you should always have the above concept in mind.

## Animation Model

Animation Model has two parts:

### Animation Object

Like DVD player, we can use it to control our animations, such as play, pause, seek, etc.

Simply put, it is the controller of the animation.

### Animation Effect

keyframes, which are the animation itself, Like DVD disc that contains the movie.

Simply put, it describes how the animation should look like by keyframes.



