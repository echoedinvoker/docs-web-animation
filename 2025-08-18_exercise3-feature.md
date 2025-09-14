# Exercise: Feature

```html
  <body>
    <header class="nav"> ... </header>
    <div class="hero"> ... </div>

    <div class="features">
      <div class="cards">
        <div class="card">
          <img src="./assets/api-interface-svgrepo-com.svg" />
          <h3>Scalability and reliability</h3>
          <p>
            The system should be able to handle growth in users and data without
            compromising performance or security.
          </p>
        </div>
        <div class="card">
          <img src="./assets/safe-and-stable-svgrepo-com.svg" />
          <h3>Security and data protection</h3>
          <p>
            Robust security measures are essential to protect user data and
            privacy.
          </p>
        </div>
        <div class="card">
          <img src="./assets/touch-click-svgrepo-com.svg" />
          <h3>Accessibility features</h3>
          <p>Ensuring the product is usable by people with disabilities.</p>
        </div>
        <div class="card">
          <img src="./assets/page-analysis-svgrepo-com.svg" />
          <h3>Data analytics and insights</h3>
          <p>
            Tools to collect, analyze, and visualize data to gain valuable
            insights and inform decision-making.
          </p>
        </div>
        <div class="card">
          <img src="./assets/dns-svgrepo-com.svg" />
          <h3>Multilingual support</h3>
          <p>Catering to a global audience with language options.</p>
        </div>
      </div>
    </div>
    ...
  </body>

  ```

  ```css
.features {
  padding: 100px 0;
  background-image: linear-gradient(transparent, black, transparent);
  position: relative;
  overflow: hidden;
}

.features .cards {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 40px;
  padding: 20px 30px;
}
.features .cards .card {
  width: 100%;
  height: 100%;
  padding: 30px;
  text-align: center;
  border: 1px solid rgb(219, 76, 235, 0.5);
  background-color: rgb(50 50 50 / 23%);
  transition: background-color 0.3s;
  border-radius: 4px;
  -webkit-box-reflect: below 5px
    linear-gradient(rgba(0, 0, 0, 0), rgba(0, 0, 0, 0.15));
}
.features .cards .card:hover {
  background-color: rgb(219, 76, 235, 0.4);
}

.features .cards .card h3 {
  margin: 0 0 1rem;
  line-height: 1.6;
  font-weight: 600;
  font-size: 1.4rem;
}
.features .cards .card p {
  font-size: 1rem;
  line-height: 1.6;
  opacity: 0.7;
}
.features .cards .card img {
  width: 100%;
  max-width: 100px;
  margin-bottom: 1rem;
}
```


The above is the content of the features section, without any animation effects, just static cards.


## Create some variables for the number of cards and their index

Because there will be some complex animation calculations based on the number of cards and index (forward, backward) later, so define some variables first.

```css
.features { ... }

.features .cards {
  --num: 5;
  ...
}
.features .cards .card {
  --j: calc(var(--num) - var(--i));
  ...
}
.features .cards .card:hover { ... }

...

.features .cards .card:nth-child(1) {
  --i: 1;
}
.features .cards .card:nth-child(2) {
  --i: 2;
}
.features .cards .card:nth-child(3) {
  --i: 3;
}
.features .cards .card:nth-child(4) {
  --i: 4;
}
.features .cards .card:nth-child(5) {
  --i: 5;
}
```


## Create an simple animation for the cards

```css
.features {
  ...
  view-timeline: --features-view; /* provides a view timeline for the animation */
}

.features .cards {
  /* ... */
  perspective: 1000px; /* adds some depth to the cards */
}
.features .cards .card {
  /* ... */

  /* apply the animation `featuresCard` to each card */
  animation: both featuresCard;
  animation-timeline: --features-view;
  animation-range: cover 20% cover 50%;
}

/* ... */

/* create the animation `featuresCard` with simple rotation effect */
@keyframes featuresCard {
  from {
    transform: rotateY(45deg);
  }
  to {
    transform: rotateY(0deg);
  }
}
```

## Shift cards left out of the screen at the start of the animation

```css
@keyframes featuresCard {
  from {
    transform: translate3d(calc(-500% - (var(--i) - 1) * 40px - 60px), 0, 0) rotateY(45deg);
    /*         ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ */
  }
  to {
    transform: translate3d(0, 0, 0) rotateY(0deg);
  }
}
```

## Add more space between cards at the start of the animation

Now the cards are too close to each other at the start of the animation like this:

![shift-cards-to-left.png](../assets/imgs/shift-cards-to-left.png)
We want to add some space between them at the start of the animation, so they look like this:

![shift-cards-with-some-space.png](../assets/imgs/shift-cards-with-some-space.png)

```css
@keyframes featuresCard {
  from {
    transform: translate3d(calc(-100% * (var(--num) + var(--j)) - (var(--i) - 1) * 40px - 60px), 0, 0) rotateY(45deg);
  }
  to {
    transform: translate3d(0, 0, 0) rotateY(0deg);
  }
}
```

## Make cards more close to the screen

```css
@keyframes featuresCard {
  from {
    transform: translate3d(
      calc(-100% * (var(--num) + var(--j)) - (var(--i) - 1) * 40px - 60px),
      0,
      calc(300px * var(--j)) /* let's move the cards closer to the screen based on the index */
    ) rotateY(45deg);
  }
  to {
    transform: translate3d(0, 0, 0) rotateY(0deg);
  }
}
```

## Add some some more rotation to the cards

Instead of using a fixed rotateY on each card, I think it would look better if there were some variation in the rotateY of each card.

```css
@keyframes featuresCard {
  from {
    transform: translate3d(
      calc(-100% * (var(--num) + var(--j)) - (var(--i) - 1) * 40px - 60px),
      0,
      calc(300px * var(--j))
    ) rotateY(calc(45deg + 60deg * (var(--i) / var(--num))));
    /*        ^^^^       ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ add some more rotation based on the card index */
  }
  to {
    transform: translate3d(0, 0, 0) rotateY(0deg);
  }
}
```
