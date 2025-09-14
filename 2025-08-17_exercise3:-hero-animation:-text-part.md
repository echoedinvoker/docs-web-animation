---
date: 2025-08-17
type: fact
aliases:
  -
hubs:
  - "[web-animation](./web-animation.md)"
---

# exercise3: hero animation: text part

```html
  <body>
    <header class="nav"> ... </header>
    <div class="hero">
      <div class="hero-inner">
        ... vidios ...

        <div class="content"> <!-- text on top of the video -->
          <div class="content-inner">
            <!-- there are two titles, but only one is visible at a time, we will use animations to switch between them -->
            <h2 class="first">Empowering progress. Through technology.</h2>
            <h2 class="second">
              Simplifying your world. One innovation at a time.
            </h2>
          </div>
        </div>
      </div>
    </div>
    ...
  </body>
```

```css
...
.hero .content {
  position: absolute;
  width: 100%;
  height: 100vh;
  display: flex;
  top: 0;
  align-items: center;
}
.hero .content .content-inner {
  width: 1000px;
  max-width: 100%;
  padding: 0 30px;
  margin: 0 auto;
}
.hero .content .content-inner h2 {
  font-size: 3.5rem;
  text-align: center;
}
```


```css
.hero .content { ... }
.hero .content .content-inner { ... }
.hero .content .content-inner h2 { ...}

/* applying the animation to the first title */
.hero .content .content-inner h2.first {
  animation: both title1;
  animation-timeline: --hero-view;
  animation-range: exit-crossing 0% exit-crossing 10%;
}
/* applying the animation to the second title */
.hero .content .content-inner h2.second {
  animation: both title2;
  animation-timeline: --hero-view;
  animation-range: exit-crossing 10% exit-crossing 70%;
}
/* creating the animation for the first title */
@keyframes title1 {
  to {
    display: none; /* this is important to remove the element from the flow at the end of the animation, or it will take up space even when not visible */
    opacity: 0;
    transform: scale(0);
  }
}
/* creating the animation for the second title */
@keyframes title2 {
  0% {
    display: none; /* this is important to remove the element from the flow at the start of the animation, or it will take up space even when not visible */
    opacity: 0;
    transform: scale(0);
  }
  30%, 70% {
    display: block;
    opacity: 1;
    transform: scale(1);
  }
  100% {
    /* here we don't need to set display: none; because the first title has already been removed from the flow, there is no chance of overlap */
    opacity: 0;
    transform: scale(0.3) rotate(5deg); /* adding a slight rotation for a more dynamic effect */
  }
}
```
