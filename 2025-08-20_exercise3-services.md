# Exercise: Services

```html
...
    <div class="services">
      <div class="video">
        <video
          loop
          muted
          autoplay
          src="./assets/1118679_map_earth_east_3840x2160.mp4"
        ></video>
      </div>
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
...

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
}

.services .cards .cards-grid .card {
  height: var(--card-height);
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
}
.services .cards .cards-grid .card-inner button:hover {
  background-color: rgba(219, 76, 235, 0.365);
}
```

## Title Appear Animation

```css
.services h2 {
  text-align: center;
  font-size: 3rem;
  margin: 0 0 4rem;
  animation: both titleAppear; /* we can use animation of other sections */
  animation-timeline: view(); /* but now we use anonymous view timeline, which based on this element itself */
}
```

## Variables for each card

```html
        <div class="cards">
          <div class="cards-grid">
            <div class="card"> 
              <div class="card-inner">
                <div class="left"> ... </div>
                <div class="right"> ... an image ... </div>

```

The two wrappers above, `.card` and `.card-inner`, are very important for creating animations later.

Three variables have already been defined in the scope of `.cards`:

```css
.services .cards {
  --card-height: 50vh;
  --card-gap: 20px;
  --num: 4;
}
```

We define the variables required for each card based on it.

```css
.services .cards .cards-grid .card {
  height: var(--card-height);
  position: sticky;
  top: var(--header-height);
}

/* Variables for each card */
.services .cards .cards-grid .card:nth-child(1) {
  --i: 1; /* index of the card */
  --r: 1deg; /* rotation of the card when it on the bottom */
  --b: var(--pink); /* border color of the card when it on the bottom */
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
```

## Try to make the cards sticky and leave space top based on the index of the card

We want to create the effect of cards stacked together, with some space above each card to see the card below.

```css
.services .cards .cards-grid .card {
  height: var(--card-height);
  position: sticky; /* make the card sticky */
  top: calc(var(--header-height) + (var(--i) * var(--card-gap))); /* leave space above based on the index of the card */
}
```

## Fix the issue of the last card not being sticky

When we scroll down, the last card does not stick to the top of the viewport. This is because there is not enough content below the last card to keep it at the top of the viewport. So we can achieve the same effect by adding padding-top to the `.card`, which will ensure that the last card still has enough space at the top of the viewport.

```css
.services .cards .cards-grid .card {
  height: var(--card-height);
  position: sticky;
  top: var(--header-height); /* keep the top position fixed */
  padding-top: calc(var(--i) * var(--card-gap)); /* add padding-top based on the index of the card */
}
```

## Fix the overflow content to the .cards-grid

Because we use padding-top on each `.card` to create spacing, it causes the height of `.cards-grid` to be insufficient, resulting in content overflow. To solve this problem, we need to add the padding-top of the last card to the padding-bottom of `.cards-grid`, so that all content can be displayed within the `.cards-grid`.

```css
.services .cards .cards-grid {
  max-width: 1000px;
  padding: 0 30px;
  margin: 0 auto;
  padding-bottom: calc(var(--num) * var(--card-gap)); /* add padding-bottom to accommodate the last card */
}

.services .cards .cards-grid .card {
  height: var(--card-height);
  position: sticky;
  top: var(--header-height);
  padding-top: calc(var(--i) * var(--card-gap));
}
```

## Infinite Glow Effect for the Button of each Card

```css
.services .cards .cards-grid .card-inner button {
  background-color: rgba(219, 76, 235, 0.146);
  border: 2px solid var(--pink);
  font-size: 0.9rem;
  padding: 10px 15px;
  color: #fff;
  cursor: pointer;
  border-radius: 5px;
  transition: 0.3s;
  animation: 2s both infinit buttonGlow; /* apply the glow effect infinite */
}
.services .cards .cards-grid .card-inner button:hover { ... }

/* create a glow effect for the button */
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
