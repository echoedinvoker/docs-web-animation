# Exercise: Team Background Logo

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
  height: 360vh;
  view-timeline: --team-view;
}
.team .team-inner {
  height: calc(100vh - var(--header-height));
  position: sticky;
  top: var(--header-height);
  overflow-x: hidden;
  animation: both teamBg;
  animation-timeline: --team-view;
  animation-range: contain;
}
.team .team-inner .team-grid {
  width: 180vw;
  height: 100%;
  overflow: hidden;
  display: flex;
  align-items: center;
  animation: both moveTeamGrid;
  animation-timeline: --team-view;
  animation-range: contain;
}
.team .team-inner h2 {
  position: absolute;
  text-align: center;
  font-size: 3rem;
  left: 0;
  right: 0;
  margin: auto;
  top: 10%;
  animation: both titleAppear;
  animation-timeline: --team-view;
  animation-range: entry-crossing 20vh entry-crossing 40vh;
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
  animation: both teamAppear;
  animation-timeline: --team-view;
  animation-range: entry-crossing 40vh entry-crossing 80vh;
}
.team .team-inner .team-grid .grid .card img {
  width: 90%;
  max-width: 130px;
  aspect-ratio: 1;
  border-radius: 50%;
  margin-bottom: 20px;
  outline: 1px solid var(--green);
  outline-offset: 3px;
  animation: both rotateTeamMemeberImage;
  animation-timeline: --team-view;
  animation-range: contain;
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

In the example above, animations have already been added to the content of the team card. Now we want to add animation effects to its background logo as well.

## Create/apply the neon animation (time based)

```css
/* ... */
.team .logo .cls-1 {
  fill: transparent;
  stroke: var(--green);
  stroke-width: 1px;
  opacity: 0.03;
  animation: 2s infinite neon; /* apply the neon effect (time based, and infinite loop) */
}

/* ... */

/* create the neon effect for bg logo */
@keyframes neon {
  0% {
    opacity: 0.03;
  }
  10% {
    opacity: 0.1;
  }
  30% {
    opacity: 0.2;
  }
  60% {
    opacity: 0.3;
  }
  100% {
    opacity: 0.1;
  }
}
```


## Create/apply the drawing animation (lead to mixed animation types)

```css
/* ... */
.team .logo .cls-1 {
  /* ... */
  stroke-dasharray: 1000; /* create a long dash to simulate the drawing effect */
  animation: 2s infinite neon, both drawLogo;
  /*                           ^^^^^^^^^^^^^ but this is based on the view timeline */
  animation-timeline: auto, --team-view; /* so we need to set the timeline for both animations */
  /*                  ^^^^ this is the default timeline, so it will animate based on the time */
}

@keyframes neon { ... }

/* create the drawing effect for bg logo */
@keyframes drawLogo {
  from {
    stroke-dashoffset: 1000;
  }
  to {
    stroke-dashoffset: 0;
  }
}
```


## Adjust the animation range

We hope that the translation animation of the team grid will start before the animation of drawing the logo, so we need to adjust the range of the animation.


```css
.team .logo .cls-1 {
  fill: transparent;
  stroke: var(--green);
  stroke-width: 1px;
  opacity: 0.03;
  stroke-dasharray: 1000;
  animation: 2s infinite neon, both drawLogo;
  animation-timeline: auto, --team-view;
  animation-range: contain 0%, contain 20%;  /* this is only for scroll progress aniamtion */
}
```
