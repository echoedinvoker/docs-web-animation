# Exercise: Hero Animation

```html
  <body>
    <header class="nav"> ... </header>
    <div class="hero">
      <div class="hero-inner"> ... </div>
    </div>
    ...
  </body>
```

```css
.hero {
  height: 100vh;
  view-timeline: --hero-view;
}
.hero-inner {
  height: 100vh;
  overflow: hidden;
}
...

```

## Create more space to scroll and stick the hero content


```css
.hero {
  height: 150vh; /* Changed from 100vh to 150vh, so we can scroll a bit */
  view-timeline: --hero-view;
}
.hero-inner {
  height: 100vh;
  overflow: hidden;

  /* use position: sticky to keep the hero-inner in view when scrolling inside the .hero */
  position: sticky;
  top: 0;
}
```

## Analyze the video

```html
      <div class="hero-inner">
        <div class="back-video">
          <video
            class="bg-video"
            src="./assets/_import_61c078b01d5ec1.54737509_1080p_12000br.mp4"
            muted
            loop
            alt=""
          ></video>
        </div>
        <div class="front-video">
          <video
            src="./assets/_import_61c078b01d5ec1.54737509_1080p_12000br.mp4"
            muted
            class="bg-video"
            loop
            alt=""
          ></video>
        </div>
        ... some text ...
    </div>
```

```css
.hero .back-video {
  position: absolute;
  top: 0;
  left: 0;
  height: 100vh;
  width: 100%;
  background-color: rgb(76, 235, 161);
  padding-top: 3px;
  padding-bottom: 3px;
  clip-path: polygon(
    47% 21%,
    100% 0,
    100% 55%,
    90% 94%,
    46% 100%,
    5% 93%,
    0% 12%
  );
}

.hero .back-video video {
  width: 100%;
  height: 100%;
  object-fit: cover;
  clip-path: polygon(
    47% 21%,
    100% 0,
    100% 55%,
    90% 94%,
    46% 100%,
    5% 93%,
    0% 12%
  );
}

.hero .front-video {
  width: 100%;
  height: 100vh;
  background-color: rgb(219, 76, 235);
  padding: 3px;
  clip-path: polygon(
    0% 0%,
    30% 0%,
    50% 15%,
    100% 15%,
    100% 80%,
    30% 100%,
    0% 50%
  );
}

.hero .front-video video {
  width: 100%;
  height: 100%;
  position: relative;
  z-index: 100;
  object-fit: cover;
  clip-path: polygon(
    0% 0%,
    30% 0%,
    50% 15%,
    100% 15%,
    100% 80%,
    30% 100%,
    0% 50%
  );
  filter: saturate(0.2);
}
```

We can see that there are two overlap videos before and after, both wrapped in containers and using the same clip-path. This allows the videos to create a polygonal effect, and padding is added to the container to give the videos a border effect.

## Front Video Animation

Now we want to add animation to the video in front, so that it changes from a polygon to a rectangle when scrolling, and changes saturation and border thickness while scrolling.

### Clip Path Animation (polygon -> rectangle -> polygon)

```css
.hero .front-video {
  /* ... */
  animation: both frontVideoClipPath;
  animation-timeline: --hero-view;
  animation-range: exit-crossing;
}

.hero .front-video video {
  /* ... */
  filter: saturate(0.2);
  animation: both frontVideoClipPath;
  animation-timeline: --hero-view;
  animation-range: exit-crossing;
}

@keyframes frontVideoClipPath {
  20% {
    clip-path: polygon(
      0% 0%,
      30% 0%,
      50% 0%, 
      /*  ^^ */
      100% 0%,
      /*   ^^ */
      100% 100%,
      /*   ^^^^ */
      30% 100%,
      0% 100%
      /* ^^^^ make the polygon a rectangle */
    );
  }
  to {
    clip-path: polygon(
      0% 0%,
      30% 0%,
      50% 0%,
      100% 0%,
      100% 90%,
      /*   ^^^ lift the bottom-left corner a bit */
      30% 100%,
      0% 90%
      /* ^^^ lift the bottom-right corner a bit */
    );
  }
}
```


### Saturation Animation (0.2 -> 1 -> 0.2)

```css
.hero .front-video { ... }

.hero .front-video video {
  /* ... */
  filter: saturate(0.2);
  animation: both frontVideoClipPath, both frontVidioSaturate;
  /*                                  ^^^^^^^^^^^^^^^^^^^^^^^ */
  animation-timeline: --hero-view;
  animation-range: exit-crossing;
}

@keyframes frontVideoClipPath { ... }

@keyframes frontVidioSaturate {
  20% {
    filter: saturate(1);
  }
  to {
    filter: saturate(0.2);
  }
}
```

### Video Border Animation

```css
.hero .front-video {
  /* ... */
  padding: 3px;
  /* ... */
  animation: both frontVideoClipPath, both border;
  /*                                  ^^^^^^^^^^^^ */
  animation-timeline: --hero-view;
  animation-range: exit-crossing;
}

.hero .front-video video { ... }

@keyframes frontVideoClipPath { ... }

@keyframes frontVidioSaturate { ... }

@keyframes border {
  0% {
    padding-top: 3px;
    padding-bottom: 3px;
    padding-left: 0;
    padding-right: 0;
  }
  20% {
    padding: 5px;
  }
  70% {
    padding-left: 0px;
    padding-right: 0px;
  }
  100% {
    padding-top: 0;
    padding-bottom: 0;
    padding-left: 3px;
    padding-right: 3px;
  }
}
```

## Back Video Animation

We increase the size of the back video while scrolling to give it a moving effect, and then fade out.

```css
.hero .back-video {
  /* ... */
  animation: both backVideo;
  animation-timeline: --hero-view;
  animation-range: exit-crossing 0% exit-crossing 20%;
  /*                             ^^               ^^^ because front video finishes at 20% */
}

.hero .back-video video {
  /* ... */
  animation: both backVideo;
  animation-timeline: --hero-view;
  animation-range: exit-crossing;
}

@keyframes backVideo {
  to {
    opacity: 0;
    transform: scale(1.2);
  }
}
```
