# Exercise: Animate Nav BG

```html
  <body>
    <header class="nav">
      <div class="content">
        <nav class="navigation" role="navigation" aria-label="Main Navigation"> ... </nav>
      </div>
    </header>
    <div class="hero"> ... </div>
    ...
  </body>
```


```css
header.nav {
  position: fixed;
  z-index: 100;
  top: 0;
  width: 100%;
  height: 100px;
}
...
.hero {
  height: 100vh;
}
...

```

## create view timeline, expend its scope and create animation based on it!

```css
.hero {
  height: 100vh;
  view-timeline: --hero-view; /* create a view timeline */
}
...
body {
  background-color: var(--bg);
  color: #fff;
  font-family: "Poppins", sans-serif;
  padding: 0;
  margin: 0;
  timeline-scope: --hero-view; /* we need to expend the scope of the view timeline  because the nav is not the child of the hero */
}
...
header.nav {
  position: fixed;
  z-index: 100;
  top: 0;
  width: 100%;
  height: 100px;

  /* use the view timeline to animation `nav` */
  animation: forwards nav;
  /*         ^^^^^^^^ to hold the last frame of the animation */
  animation-timeline: --hero-view; /* use the view timeline to animate `nav` */
  animation-range: exit-crossing /* 
}

@keyframes nav {
  to {
    height: var(--header-height);
    background-color: var(--bg);
  }
}
```

## adjust the range of animation

Based on `exit-crossing` we can further adjust the range of animation start and end to make it more sense on the page.

```css
header.nav {
  position: fixed;
  z-index: 100;
  top: 0;
  width: 100%;
  height: 100px;
  animation: forwards nav;
  animation-timeline: --hero-view;
  animation-range: exit-crossing 50% exit-crossing 70%;
  /*                             ^^^               ^^^ further adjust the range of animation start and end */
}
```
