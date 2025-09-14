---
date: 2025-08-18
type: fact
aliases:
  -
hubs:
  - "[web-animation](./web-animation.md)"
---

# exercise3: team

```html
    <div class="team">
      <div class="team-inner">
        <svg
          class="logo"
          role="img"
          focusable="false"
          xmlns="http://www.w3.org/2000/svg"
          viewBox="0 0 423.15 75"
        >
          <g>
            <path
              class="cls-1"
              d="M21.34,16.58c9.3,0,16.86,4.98,19.68,13.56h-9.66c-1.98-4.02-5.58-6-10.08-6-7.32,0-12.54,5.34-12.54,13.86s5.22,13.86,12.54,13.86c4.5,0,8.1-1.98,10.08-6.06h9.66c-2.82,8.64-10.38,13.56-19.68,13.56C9.28,59.36.1,50.54.1,38s9.18-21.42,21.24-21.42Z"
            />
            <path class="cls-1" ... />
            <path ... />
            <path ... />
            <path ... />
            <path ... />
            <path ... />
            <path ... />
            <path ... />
            <path ... />
            <path ... />
            <path ... />
          </g>
          <rect class="cls-1" x="80" y="67" width="343" height="8" />
          <rect class="cls-1" width="423" height="8" />
          <rect class="cls-1" x="0" y="67" width="59" height="8" />
        </svg>
        <h2>Our Team</h2>
        <div class="team-grid">
          <div class="grid">
            <div class="card">
              <img src="./assets/50955.jpg" alt="" />
              <h3>Maya</h3>
              <p>Software Engineer (Front-End)</p>
            </div>
            <div class="card"> ... </div>
            <div class="card"> ... </div>
            <div class="card"> ... </div>
            <div class="card"> ... </div>
            <div class="card"> ... </div>
          </div>
        </div>
      </div>
    </div>
```

```css
.team {
  position: relative;
}
.team .team-inner {
  height: calc(100vh - var(--header-height));
}
.team .team-inner .team-grid {
  height: 100%;
  overflow: hidden;
  display: flex;
  align-items: center;
}
.team .team-inner h2 {
  position: absolute;
  text-align: center;
  font-size: 3rem;
  left: 0;
  right: 0;
  margin: auto;
  top: 10%;
}

.team .team-inner .team-grid .grid {
  display: grid;
  grid-template-columns: repeat(6, 1fr);
  gap: 40px;
  padding: 20px 30px;
  align-items: center;
  justify-content: center;
  width: 100%;
}

.team .team-inner .team-grid .grid .card {
  border: 1px solid var(--green);
  height: 100%;
  background-color: rgba(32, 32, 32, 0.59);
  padding: 60px 30px;
  text-align: center;
  border-radius: 3px;
}
.team .team-inner .team-grid .grid .card img {
  width: 90%;
  max-width: 130px;
  aspect-ratio: 1;
  border-radius: 50%;
  margin-bottom: 20px;
  outline: 1px solid var(--green);
  outline-offset: 3px;
}
.team .team-inner .team-grid .grid .card h3 {
  font-size: 1.4rem;
  margin: 0 0 15px;
}
.team .team-inner .team-grid .grid .card p {
  font-size: 0.9rem;
  opacity: 0.7;
  margin: 0;
}

.team .logo {
  width: 96%;
  position: absolute;
  left: 2%;
  top: 0%;
  bottom: 0;
  margin: auto;
}
.team .logo .cls-1 {
  fill: transparent;
  stroke: var(--green);
  stroke-width: 1px;
  opacity: 0.03;
}
```

The above is the content of the team section, it is a static HTML and CSS.

## Add hieght to scroll

```css
.team {
  position: relative;
  height: 360vh; /* make it higher than the viewport */
}
.team .team-inner {
  height: calc(100vh - var(--header-height));

  /* sticky to content of the team section when scrolling on the team section */
  position: sticky;
  top: var(--header-height);
}
...

```

## Expand the grid wider than the viewport

Because our animation scrolls on the y-axis, the team-grid will move from right to left, so we need to make the width of the team-grid greater than the width of the viewport in order to see the animation effect.

```css
.team { ... }
.team .team-inner {
  /* ... */
  overflow-x: hidden; /* because the grid is wider than the viewport, we need to hide the overflow on the x-axis */
}
.team .team-inner .team-grid {
  width: 180vw; /* make it wider than the viewport */
  /* ... */
}
/* ... */

```

## Create Grid Shifting, Background Color Change, and Image Rotation Animations


```css
.team {
  /* ... */
  view-timeline: --team-view;
}
.team .team-inner {
  /* ... */

  /* apply the animation to the background */
  animation: both teamBg;
  animation-timeline: --team-view;
  animation-range: contain;
}
.team .team-inner .team-grid {
  /* ... */
  
  /* apply the animation to the grid */
  animation: both moveTeamGrid;
  animation-timeline: --team-view;
  animation-range: contain;
}
.team .team-inner h2 { ... }
.team .team-inner .team-grid .grid { ... }
.team .team-inner .team-grid .grid .card { ... }
.team .team-inner .team-grid .grid .card img {
  /* ... */

  /* apply the animation to the team member image */
  animation: both rotateTeamMemeberImage;
  animation-timeline: --team-view;
  animation-range: contain;
}

/* ... */

/* create the animations for the background, grid movement, and image rotation */
@keyframes moveTeamGrid {
  to {
    transform: translateX(calc(-100% + 100vw));
  }
}
@keyframes rotateTeamMemeberImage {
  to {
    transform: rotateY(360deg);
  }
}
@keyframes teamBg {
  0% {
    background-color: var(--bg);
  }
  10%, 80% {
    background-color: #0a120f;
  }
  100% {
    background-color: var(--bg);
  }
}
```

## Animations of appearing title and team members

```css
.team {
  /* ... */
  view-timeline: --team-view;
}
.team .team-inner { ... }
.team .team-inner .team-grid { ... }
.team .team-inner h2 {
  /* ... */

  /* apply the animation to the title */
  animation: both titleAppear;
  animation-timeline: --team-view;
  animation-range: entry-crossing 20vh entry-crossing 40vh;
}

.team .team-inner .team-grid .grid { ... }
.team .team-inner .team-grid .grid .card {
  /* ... */

  /* apply the animation to the team members */
  animation: both teamAppear;
  animation-timeline: --team-view;
  animation-range: entry-crossing 40vh entry-crossing 80vh;
}

/* ... */

@keyframes moveTeamGrid { ... }
@keyframes rotateTeamMemeberImage { ... }
@keyframes teamBg { ... }

/* create the animations for the title and team appearance */
@keyframes titleAppear {
  0% {
    opacity: 0;
    transform: translateY(-30px);
  }
  100% {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes teamAppear {
  0% {
    opacity: 0;
    clip-path: inset(10%);
  }
  100% {
    opacity: 1;
    clip-path: inset(0);
  }
}
```
