# Animation Exercise 1

Below is a simple pre-work for animation practice (not including animation yet), including HTML and CSS structure with elements such as sky, road, characters, etc. The character uses an 8-frame sprite for future animation implementation. The purpose of this exercise is to familiarize students with how to use CSS to design and layout a simple animated scene.


```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="stylesheet" href="styles.css" />
    <title>Animations Exercise</title>
    <script>
      document.addEventListener("DOMContentLoaded", () => {});
    </script>
  </head>
  <body>
    <div class="view">

      <div class="sky"> <!-- Sky background with clouds -->
        <img src="assets/cloud1.png" class="cloud1" alt="" />
        <img src="assets/cloud2.png" class="cloud2" alt="" />
      </div>

      <div class="background"> <!-- Road sign on top of the road -->
        <img src="assets/s1.png" alt="" />
      </div>

      <div class="ground"> <!-- Ground with street lines -->
        <div class="street">
          <div class="lines"></div>
        </div>
      </div>

      <div class="character"></div> <!-- Character that we want to animate -->

      <div class="foreground"> <!-- Road sign on the road (bottom) -->
        <img src="assets/s2.png" alt="" />
      </div>

    </div>
  </body>
</html>
```

```css
body {
  background-color: #1d1d1d;
  font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
    Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
  padding: 0;
  margin: 0;
}

* {
  box-sizing: border-box;
}
/* above is basic reset */

.sky {
  background-image: linear-gradient(rgb(97, 191, 228), rgb(204, 233, 245));
  height: 50vh; /* 50% of the viewport height, so it is flexible to different screen heights */
  position: relative;
}
/* use absolute positioning for positioning clouds in the sky */
.sky .cloud1 {
  position: absolute;
  height: 30%;
  top: 10%;
  left: 10%;
}
.sky .cloud2 {
  position: absolute;
  height: 30%;
  top: 20%;
  right: 10%;
}

.ground {
  height: 50vh; /* 50% of the viewport height, so it is flexible to different screen heights */
                /* and it will be positioned below the sky (same document flow) */
  position: relative;
}
.ground .street {
  background-image: linear-gradient(#464646, #242424); /* Dark gray for the road */
  position: absolute;
  /* Completely fill the ground box, and the width is 200 viewport width for later animation presentation */
  width: 200vw;
  height: 100%;
}
.ground .street .lines { /* lines are inside the street, so they will be positioned relative to the street */
  position: absolute;
  background-image: url("./assets/line.png");
  background-size: 12.5% auto; /* 12.5% of the width of the street (small than the width of the street) */ 
  background-repeat: repeat-x; /* Repeat the line image horizontally (for multiple lines) */
  background-position: 0 0;
  height: 4%;
  width: 100%;
  top: 50%;
  opacity: 0.8;
}

.character {

  /* absolute positioning with all sides set to 0 and then simply use margin auto to center it */
  position: absolute;
  bottom: 0;
  top: 0;
  left: 0;
  right: 0;
  margin: auto;

  background-image: url("./assets/7888220.png");

  /* because the character is a sprite of 8 frames, we set `--char-width` to 25vh as width of single frame */
  /* and then use it to calculate the background size (8 frames * width of single frame) */
  --char-width: 25vh;
  width: var(--char-width);
  background-size: calc(var(--char-width) * 8);
  background-repeat: no-repeat;
  /* background position to select which frame to show */
  background-position: calc(var(--char-width) * -4) 0px;
  /*                                            ^^ can be changed to -7, -6, -5, -4, -3, -2, -1, 0 to show different frames */
  aspect-ratio: 0.535; /* fixed aspect ratio for the character image, different images may require different aspect ratios */
}

/* Below is the style for road signs, simply using absolute positioning. */
.background {
  position: absolute;
  height: 50vh;
  width: 100%;
  top: 0;
}
.background img {
  position: absolute;
  height: 50%;
  bottom: -5px;
  left: 0;
}
.foreground {
  position: absolute;
  height: 50vh;
  width: 100%;
  top: 50%;
}
.foreground img {
  position: absolute;
  height: 90%;
  bottom: 0px;
  left: 0;
}
```
