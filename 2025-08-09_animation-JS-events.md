# animation JS events

```css
.square {
  width: 100px;
  height: 100px;
  background-color: gold;
  animation: move 2s ease-in-out 3 alternate;
  animation-fill-mode: forwards;
}

@keyframes move {
  0% {
    transform: translateX(0);
  }
  100% {
    transform: translateX(calc(100vw - 140px));
  }
}
```

Like transitions, animations also have events that can be listened to.

```js
      document.addEventListener("DOMContentLoaded", () => {
        const square = document.querySelector(".square");

        square.addEventListener("animationstart", (e) => {
          console.log("Animation started:", e);
        });
        square.addEventListener("animationend", (e) => {
          console.log("Animation ended:", e);
        });
        square.addEventListener("animationiteration", (e) => {
          console.log("Animation iteration:", e);
        });
        square.addEventListener("animationcancel", (e) => {
          console.log("Animation cancelled:", e);
        });
      });
```

Let's play with the animation and see how the events are logged in the console.

```
Animation started:
    AnimationEvent {
        isTrusted: true
        animationName: "move"
        bubbles: true
        cancelBubble: false
        cancelable: true
        composed: false
        currentTarget: null
        defaultPrevented: false
        elapsedTime: 0
        eventPhase: 0
        pseudoElement: ""
        returnValue: true
        srcElement: div.square
        target: div.square
        timeStamp: 1407
        type: "animationstart"
        [Prototype](./Prototype.md): AnimationEvent
    }
Animation iteration:
    AnimationEvent {
        // ... omitting some properties for brevity, only focusing on the key ones
        animationName: "move"
        elapsedTime: 2 <------------------ because we set each iteration to 2s
        srcElement: div.square
        target: div.square
        timeStamp: 3391.199999999255
        type: "animationiteration"
    }
Animation iteration:
    AnimationEvent {
        // ... omitting some properties for brevity, only focusing on the key ones
        animationName: "move"
        elapsedTime: 4 <------------------ because we set each iteration to 2s, and now it's the second iteration
        srcElement: div.square
        target: div.square
        type: "animationiteration"
    }
Animation ended:
    AnimationEvent {
        // ... omitting some properties for brevity, only focusing on the key ones
        animationName: "move"
        elapsedTime: 6 <------------------ because we set each iteration to 2s, and now it's the third iteration
        srcElement: div.square
        target: div.square
        type: "animationend"
    }
```


It can be observed that the event `animationcancel` is not triggered when the animation is played in full. We can use browser dev tools to remove the animation style while the animation is running.

```
Animation started:
    AnimationEvent {
        // ...
        animationName: "move"
        elapsedTime: 0
        srcElement: div.square
        target: div.square
        type: "animationstart"
    }

// after the animation has started, we remove the animation style from the element with dev tools...

Animation cancelled:
    AnimationEvent {
        // ...
        animationName: "move"
        elapsedTime: 0
        srcElement: div.square
        target: div.square
        type: "animationcancel" <-- because we removed the animation style
    }
```


