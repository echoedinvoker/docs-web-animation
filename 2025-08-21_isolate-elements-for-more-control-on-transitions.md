# Isolate Elements for More Control on Transitions

```html
  <body>
    <div class="header"> ... </div>
    <div class="grid-outer"> ... </div>
  </body>
```


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

## Isolating elements from root pseudo-element

The transition animation above transitions the entire page, but sometimes we do not want to transition the entire page. Instead, certain elements may not transition or may have different transition effects. In this case, we can isolate the elements.


```css
.header {
  background-color: #0c0d12;
  position: sticky;
  top: 0;
  z-index: 100;
  view-transition-name: header; /* Isolate this element for transitions */
}
```


```html
<html ...>
  ::view-transition
    ::view-transition-group(root)
    ::view-transition-group(header) <-- isolate header to a pseudo-element from root
```

Above, the header is isolated as another pseudo-element `::view-transition-group(header)`, and by default, it uses the crossfade animation. If we do not want any animation on the header, we can override it as follows.

```css
::view-transition-old(header), ::view-transition-new(header) {
  animation: none;
}
```

## How about isolating the element which has different size or position?


```css
.header .title {
  view-transition-name: header-title;
}
```

```html
<html ...>
  ::view-transition
    ::view-transition-group(root)
    ::view-transition-group(header)
    ::view-transition-group(header-title) <-- isolate title to a pseudo-element from header
```


```
html::view-transition-group(header-title) {

    // size and position of the new state
    width: 345.562px;
    height: 79.625px;
    transform: matrix(1, 0, 0, 1, 67.7778, 0);

    color-scheme: normal;
    text-orientation: mixed;
    writing-mode: horizontal-tb;
    backdrop-filter: none;
    mix-blend-mode: normal;
    animation-name: -ua-view-transition-group-anim-header-title;
    //              ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
    animation-timing-function: ease;
    animation-delay: 0s;
    animation-iteration-count: 1;
    animation-direction: normal;
}

@keyframes -ua-view-transition-group-anim-header-title {
   0% {
        // size and position of the old state
        transform: matrix(1, 0, 0, 1, 307.778, 0);
        width: 345.562px;
        height: 79.625px;
        backdrop-filter: none;
   }
}
```

Above, we can see that the animation is applied to `::view-transition-group(header-title)`, and the animation is not the effect of `fade-in`, `fade-out`, and `mix-blend-mode`, but rather the changes in element size and position.


## Overriding the transition animation on the `::view-transition-group`

When the size or position of isolated elements changes, I should override the animation of `::view-transition-group` instead of `::view-transition-old` and `::view-transition-new`.


```css
::view-transition-group(header-title) {
  animation-duration: 5s;
}
```


## View Transition Name should be UNIQUE

As long as `view-transition-name` is unique, a transition animation can be generated, even if the DOM elements before and after are not the same.

If multiple identical `view-transition-name` are set, it will cause an error because the View Transition API cannot determine which ones are grouped as the start and end of the transition.

```html
