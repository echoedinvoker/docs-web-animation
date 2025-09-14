---
date: 2025-08-17
type: fact
aliases:
  -
hubs:
  - "[web-animation](./web-animation.md)"
---

# exercise3: animate logo

```html
  ...
  <body>
    <header class="nav">
      <div class="content">
        <h1>
          <a href="/">
            <svg
              id="logo"
              role="img"
              focusable="false"
              aria-labelledby="svg-logo-title svg-logo-desc"
              xmlns="http://www.w3.org/2000/svg"
              viewBox="0 0 423.15 75"
            >
              <title id="svg-logo-title">CipherStream</title>
              <desc id="svg-logo-desc">
                The logo is comprised of the text CipherStream.
              </desc>
              <g>
                <path
                  class="cls-1"
                  d="M21.34,16.58c9.3,0,16.86,4.98,19.68,13.56h-9.66c-1.98-4.02-5.58-6-10.08-6-7.32,0-12.54,5.34-12.54,13.86s5.22,13.86,12.54,13.86c4.5,0,8.1-1.98,10.08-6.06h9.66c-2.82,8.64-10.38,13.56-19.68,13.56C9.28,59.36.1,50.54.1,38s9.18-21.42,21.24-21.42Z"
                />
                <path
                  class="cls-1"
                  d="M47.32,16.88c0-2.76,2.16-4.92,5.16-4.92s5.16,2.16,5.16,4.92-2.22,4.92-5.16,4.92-5.16-2.16-5.16-4.92ZM48.22,25.76h8.4v33.24h-8.4V25.76Z"
                />
                <path
                  class="cls-1"
                  d="M84.28,25.22c8.52,0,15.18,6.66,15.18,17.04s-6.66,17.28-15.18,17.28c-5.22,0-8.94-2.58-10.98-5.28v20.58h-8.4V25.76h8.4v4.8c1.98-2.82,5.82-5.34,10.98-5.34ZM82.06,32.6c-4.5,0-8.76,3.48-8.76,9.78s4.26,9.78,8.76,9.78,8.82-3.6,8.82-9.9-4.26-9.66-8.82-9.66Z"
                />
                <path
                  class="cls-1"
                  d="M105.58,14.6h8.4v15.3c2.16-2.82,5.88-4.62,10.32-4.62,7.5,0,12.96,5.04,12.96,14.22v19.5h-8.4v-18.36c0-5.34-2.94-8.22-7.38-8.22s-7.5,2.88-7.5,8.22v18.36h-8.4V14.6Z"
                />
                <path
                  class="cls-1"
                  d="M159.76,59.54c-9.66,0-16.68-6.72-16.68-17.16s6.84-17.16,16.68-17.16,16.38,6.54,16.38,16.44c0,1.08-.06,2.16-.24,3.24h-24.3c.42,4.92,3.78,7.68,7.98,7.68,3.6,0,5.58-1.8,6.66-4.02h9.06c-1.8,6.12-7.32,10.98-15.54,10.98ZM151.66,39.26h15.78c-.12-4.38-3.6-7.14-7.92-7.14-4.02,0-7.2,2.58-7.86,7.14Z"
                />
                <path
                  class="cls-1"
                  d="M190.66,59h-8.4V25.76h8.4v5.16c2.1-3.42,5.58-5.64,10.2-5.64v8.82h-2.22c-4.98,0-7.98,1.92-7.98,8.34v16.56Z"
                />
                <path
                  class="cls-1"
                  d="M220.96,59.42c-8.7,0-15.42-4.56-15.54-12.48h9c.24,3.36,2.46,5.58,6.36,5.58s6.3-2.1,6.3-5.1c0-9.06-21.6-3.6-21.54-18.78,0-7.56,6.12-12.12,14.76-12.12s14.46,4.38,15,11.94h-9.24c-.18-2.76-2.4-4.92-6-4.98-3.3-.12-5.76,1.5-5.76,4.92,0,8.4,21.48,3.72,21.48,18.48,0,6.6-5.28,12.54-14.82,12.54Z"
                />
                <path
                  class="cls-1"
                  d="M244.36,32.66h-3.96v-6.9h3.96v-8.22h8.46v8.22h7.44v6.9h-7.44v16.08c0,2.22.9,3.18,3.54,3.18h3.9v7.08h-5.28c-6.36,0-10.62-2.7-10.62-10.32v-16.02Z"
                />
                <path
                  class="cls-1"
                  d="M274.72,59h-8.4V25.76h8.4v5.16c2.1-3.42,5.58-5.64,10.2-5.64v8.82h-2.22c-4.98,0-7.98,1.92-7.98,8.34v16.56Z"
                />
                <path
                  class="cls-1"
                  d="M305.08,59.54c-9.66,0-16.68-6.72-16.68-17.16s6.84-17.16,16.68-17.16,16.38,6.54,16.38,16.44c0,1.08-.06,2.16-.24,3.24h-24.3c.42,4.92,3.78,7.68,7.98,7.68,3.6,0,5.58-1.8,6.66-4.02h9.06c-1.8,6.12-7.32,10.98-15.54,10.98ZM296.98,39.26h15.78c-.12-4.38-3.6-7.14-7.92-7.14-4.02,0-7.2,2.58-7.86,7.14Z"
                />
                <path
                  class="cls-1"
                  d="M340.59,25.22c5.34,0,9,2.52,10.98,5.28v-4.74h8.46v33.24h-8.46v-4.86c-1.98,2.88-5.76,5.4-11.04,5.4-8.4,0-15.12-6.9-15.12-17.28s6.72-17.04,15.18-17.04ZM342.75,32.6c-4.5,0-8.76,3.36-8.76,9.66s4.26,9.9,8.76,9.9,8.82-3.48,8.82-9.78-4.2-9.78-8.82-9.78Z"
                />
                <path
                  class="cls-1"
                  d="M414.75,40.64c0-5.28-2.94-8.04-7.38-8.04s-7.44,2.76-7.44,8.04v18.36h-8.4v-18.36c0-5.28-2.94-8.04-7.38-8.04s-7.5,2.76-7.5,8.04v18.36h-8.4V25.76h8.4v4.02c2.1-2.76,5.64-4.5,9.78-4.5,5.16,0,9.42,2.22,11.7,6.36,2.16-3.78,6.54-6.36,11.4-6.36,7.98,0,13.62,5.04,13.62,14.22v19.5h-8.4v-18.36Z"
                />
              </g>
              <rect class="cls-1" x="80" y="67" width="343" height="8" />
              <rect class="cls-1" width="423" height="8" />
              <rect class="cls-1" x="0" y="67" width="59" height="8" />
            </svg>
          </a>
        </h1>
        <nav class="navigation" role="navigation" aria-label="Main Navigation"> ... </nav>
      </div>
    </header>
    ...
  </body>
```

```css
header.nav h1 svg {
  width: 220px;
  aspect-ratio: 5;
  height: auto;
}

header.nav h1 svg .cls-1 {
  fill: rgba(255, 255, 255, 1);
}
```

The SVG logo above uses complete codes instead of using the import method, so that we can add animation effects to the path. Next, we will add animation effects.

```html
            <svg
              id="logo"
              class="animate" <-- add this class, because the animation is not always running, we can control it with this class
              role="img"
              focusable="false"
              aria-labelledby="svg-logo-title svg-logo-desc"
              xmlns="http://www.w3.org/2000/svg"
              viewBox="0 0 423.15 75"
            >
              <title id="svg-logo-title">CipherStream</title>
              <desc id="svg-logo-desc">
                The logo is comprised of the text CipherStream.
              </desc>
              <g>
                <path
                  class="cls-1"
                  d="M21.34,16.58c9.3,0,16.86,4.98,19.68,13.56h-9.66c-1.98-4.02-5.58-6-10.08-6-7.32,0-12.54,5.34-12.54,13.86s5.22,13.86,12.54,13.86c4.5,0,8.1-1.98,10.08-6.06h9.66c-2.82,8.64-10.38,13.56-19.68,13.56C9.28,59.36.1,50.54.1,38s9.18-21.42,21.24-21.42Z"
                />
                ...
              </g>
              <rect class="cls-1" x="80" y="67" width="343" height="8" />
              <rect class="cls-1" width="423" height="8" />
              <rect class="cls-1" x="0" y="67" width="59" height="8" />
            </svg>
```


```css
header.nav h1 svg { ... }

header.nav h1 svg .cls-1 {
  fill: rgba(255, 255, 255, 1);
}

header.nav h1 svg.animate .cls-1 {
/*               ^^^^^^^^ when there is the class "animate", the animation will be applied */
  fill: rgba(255, 255, 255, 0); /* make the fill transparent initially when animating */

  /* animate the stroke */
  stroke: var(--green);
  stroke-width: 2;
  stroke-dasharray: 1000; /* large enough to cover the entire path */
  animation: 3s ease-in-out drawLogo;
}

/* create the animation to draw the logo with stroke */
@keyframes drawLogo {
  0% {
    stroke-dashoffset: 1000; /* large enough to cover the entire path, so the animation will be looking like it's drawing the logo */
  }
  100% {
    stroke-dashoffset: 0;
  }
}
```

Now, the animation is already working, but it will only run once and not return to the original state. To make the animation repeat every 5 seconds and return to the original state between animations, we can use JavaScript to control the animation.

```js
document.addEventListener("DOMContentLoaded", () => {
  const logoSVG = document.getElementById("logo"); // `.animate` class of the logo SVG decides the animation work or not
  const logoPath = logoSVG.querySelector(".cls-1"); // our animation is on the path with class "cls-1"
  logoPath.addEventListener("animationend", () => {
    logoSVG.classList.remove("animate"); // stop the animation after it completes
    setTimeout(() => {
      logoSVG.classList.add("animate"); // restart the animation
    }, 5000); // repeat animation every 5 seconds
  });
});
```
