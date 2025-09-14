# Exercise: Services 2

```html
    <div class="services">
      <div class="video"> ... </div>
      <div class="content">
        <h2>Our Services</h2>

        <div class="cards">
          <div class="cards-grid">
            <div class="card">
              <div class="card-inner">
                <div class="left">
                  <h3>Software development</h3>
                  <p>
                    Creating custom software applications, mobile apps, or web
                    applications for various purposes.
                  </p>
                  <button>Explore More</button>
                </div>
                <div class="right">
                  <img src="./assets/api-interface-svgrepo-com.svg" alt="" />
                </div>
              </div>
            </div>
            <div class="card">
              <div class="card-inner">
                <div class="left">
                  <h3>Software as a Service (SaaS)</h3>
                  <p>
                    Providing subscription-based access to cloud-hosted software
                    for tasks like project management, customer relationship
                    management (CRM), or marketing automation.
                  </p>
                  <button>Explore More</button>
                </div>
                <div class="right">
                  <img src="./assets/dns-svgrepo-com.svg" alt="" />
                </div>
              </div>
            </div>
            <div class="card">
              <div class="card-inner">
                <div class="left">
                  <h3>Platform as a Service (PaaS)</h3>
                  <p>
                    Offering a platform for developers to build and deploy their
                    own applications.
                  </p>
                  <button>Explore More</button>
                </div>
                <div class="right">
                  <img src="./assets/page-analysis-svgrepo-com.svg" alt="" />
                </div>
              </div>
            </div>
            <div class="card">
              <div class="card-inner">
                <div class="left">
                  <h3>Maintenance and support</h3>
                  <p>
                    Providing ongoing maintenance, bug fixes, and technical
                    support for software products.
                  </p>
                  <button>Explore More</button>
                </div>
                <div class="right">
                  <img src="./assets/safe-and-stable-svgrepo-com.svg" alt="" />
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
```


```css
.services {
  padding: 20px 0px 60px;
  position: relative;
  background-image: linear-gradient(transparent, black);
}

.services .video {
  position: sticky;
  top: 0;
  width: 100%;
  height: 100vh;
  mask-image: linear-gradient(transparent, black, transparent);
}
.services .video video {
  width: 100%;
  height: 100%;
  object-fit: cover;
  filter: hue-rotate(105deg) brightness(0.3);
}

.services .content {
  margin-top: -100vh;
}
.services h2 {
  text-align: center;
  font-size: 3rem;
  margin: 0 0 4rem;
  animation: both titleAppear;
  animation-timeline: view();
}
.services .cards {
  --card-height: 50vh;
  --card-gap: 20px;
  --num: 4;
}

.services .cards .cards-grid {
  max-width: 1000px;
  padding: 0 30px;
  margin: 0 auto;
  padding-bottom: calc(var(--num) * var(--card-gap));
}

.services .cards .cards-grid .card {
  height: var(--card-height);
  position: sticky;
  top: var(--header-height);
  padding-top: calc(var(--i) * var(--card-gap));
}
.services .cards .cards-grid .card:nth-child(1) {
  --i: 1;
  --r: 1deg;
  --b: var(--pink);
}
.services .cards .cards-grid .card:nth-child(2) {
  --i: 2;
  --r: -1deg;
  --b: #48fbfb;
}
.services .cards .cards-grid .card:nth-child(3) {
  --i: 3;
  --r: -1deg;
  --b: #e7e64d;
}
.services .cards .cards-grid .card:nth-child(4) {
  --i: 4;
  --r: 0deg;
}
.services .cards .cards-grid .card-inner {
  background-color: rgb(11 9 14 / 85%);
  backdrop-filter: blur(6px);
  border: 2px solid var(--green);
  height: var(--card-height);
  border-radius: 5px;
  display: flex;
  align-items: center;
  padding: 30px;
}

.services .cards .cards-grid .card-inner .left {
  flex: 1;
  width: 50%;
}
.services .cards .cards-grid .card-inner .right {
  flex: 1;
}
.services .cards .cards-grid .card-inner img {
  width: 100%;
  object-fit: contain;
}
.services .cards .cards-grid .card-inner h3 {
  font-size: 2.4rem;
  line-height: 1.4;
  margin: 0 0 1rem;
}
.services .cards .cards-grid .card-inner p {
  font-size: 1.1rem;
  line-height: 1.6;
  opacity: 0.8;
  margin-bottom: 40px;
}
.services .cards .cards-grid .card-inner button {
  background-color: rgba(219, 76, 235, 0.146);
  border: 2px solid var(--pink);
  font-size: 0.9rem;
  padding: 10px 15px;
  color: #fff;
  cursor: pointer;
  border-radius: 5px;
  transition: 0.3s;
  animation: 2s both infinite buttonGlow;
}
.services .cards .cards-grid .card-inner button:hover {
  background-color: rgba(219, 76, 235, 0.365);
}

@keyframes buttonGlow {
  0% {
    box-shadow: 0 0 2px var(--pink);
  }
  50% {
    background-color: rgba(219, 76, 235, 0.2);
    box-shadow: 0 0 10px var(--pink);
  }
  100% {
    box-shadow: 0 0 2px var(--pink);
  }
}
```


```css
.services .cards .cards-grid .card-inner {
  background-color: rgb(11 9 14 / 85%);
  backdrop-filter: blur(6px);
  border: 2px solid var(--green);
  height: var(--card-height);
  border-radius: 5px;
  display: flex;
  align-items: center;
  padding: 30px;
  view-timeline: --card-view; /* Define a view timeline for each card */
}

/* ... */

.services .cards .cards-grid .card-inner img {
  width: 100%;
  object-fit: contain;
  animation: both cardImage; /* apply the animation */
  animation-timeline: --card-view;  /* use the view timeline defined above */
}

/* ... */

/* create an animation for the card image */
@keyframes cardImage {
  0% {
    transform: scale(0);
  }
  10% {
    transform: scale(1.3);
  }
  100% {
    transform: scale(1);
  }
}
```

## Main animation for the card


```css
.services .cards {
  --card-height: 50vh;
  --card-gap: 20px;
  --num: 4;
  view-timeline: --cards-view; /* provides a view timeline for the entire cards container */
}                              /* it means we need to define different animation ranges for each card based on their index */

.services .cards .cards-grid .card-inner {
  background-color: rgb(11 9 14 / 85%);
  backdrop-filter: blur(6px);
  border: 2px solid var(--green);
  height: var(--card-height);
  border-radius: 5px;
  display: flex;
  align-items: center;
  padding: 30px;
  transform-origin: 50% 0; /* Set the transform origin to the top center, so when the transform: rotate and scale are applied, they will be centered at the top of the card and not overlap the next card */
  view-timeline: --card-view;

  /* apply the animation to each card */
  animation: both serviceCard;
  animation-timeline: --cards-view; /* use the view timeline provided by the cards container */

  /* calculate the animation range for each card based on its index */
  animation-range: exit-crossing calc(((var(--i) - 1) / var(--num)) * 100%)
    exit-crossing calc((var(--i) / var(--num)) * 100%);
}

/* ... */

/* create an animation for each card when they are on the bottom (exit-crossing) */
@keyframes serviceCard {
  to {
    border-color: var(--b);
    filter: blur(calc(var(--num) - var(--i)) * 1px); /* apply a blur effect based on the index of the card */
    transform: rotate(var(--r)) /* rotate the card based on each card's variable */
      scale(calc(1 - (0.05 * (var(--num) - var(--i))))); /* scale the card based on the index of the card */
  }
}
```
