---
date: 2025-08-21
type: fact
aliases:
  -
hubs:
  - "[web-animation](./web-animation.md)"
---

# Custom default crossfade transition

```html
<html ...>
  ::view-transition
    ::view-transition-group(root) <-- responsible for changing the size and the position of the elements
      ::view-transition-image-pair(root)
        ::view-transition-old(root) <-- screenshot of the old state
        ::view-transition-new(root) <-- screenshot of the new state
```

The default transition is from `::view-transition-old(root)` using a crossfade animation (opacity 0 to 1 and mix-blend-mode) to `::view-transition-new(root)`.

However, we can override this default transition animation property in CSS as follows:

```css
::view-transition-old(root), ::view-transition-new(root) {
  animation-duration: 5s;
}
```

However, please note that during the transition process, the user cannot interact with the page. This is because during the transition process, the entire page is covered by pseudo elements, so we usually hope that the transition time is not too long.

Instead of fade in/out, we can also use a slide in/out animation. This is done by creating a custom animations and applying them to the `::view-transition-old(root)` and `::view-transition-new(root)` pseudo-elements.

```css
::view-transition-old(root) {
  animation: 0.5s both ease-in-out slideOut;
}
::view-transition-new(root) {
  animation: 0.5s both ease-in-out slideIn;
}

@keyframes slideOut {
  to {
    transform: translateX(-100%);
  }
}
@keyframes slideIn {
  from {
    transform: translateX(100%);
  }
  to {
    transform: translateX(0);
  }
}
```

We all start from `::view-transition-old(root)` and then move on to `::view-transition-new(root)`. So it's the transition effect of the entire page, but sometimes we want certain elements on the page to have different transition effects, which will be introduced in other topics.


