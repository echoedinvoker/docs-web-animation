---
date: 2025-08-07
type: fact
aliases:
  -
hubs:
  - "[web-animation](./web-animation.md)"
---

# Transitions

## Basic

```css
.square {
  width: 200px;
  height: 200px;
  background-color: purple;
  transition-property: transform; /* Specify the property to transition */
  transition-duration: 1s; /* Duration of the transition */
  transition-delay: 0.5s; /* Delay before the transition starts */
  transition-timing-function: ease-in-out; /* Timing function for the transition, here we use ease-in-out predefined function */
}

.square:hover {
  transform: rotate(45deg); /* Rotate the square by 45 degrees on hover */
}
```

Note that not all properties can be transitioned, as the browser needs to know how to *interpolate the values between the start and end states*. Some properties that cannot be transitioned are simply because the browser does not know how to interpolate, such as the writing property.

We can shorten the transition related properties into a single `transition` property:

```css
.square {
  width: 200px;
  height: 200px;
  background-color: purple;
  transition: 1s 0.5s transform ease-in-out; /* Shorthand for transition properties */
                                             /* 1s duration, 0.5s delay, transform property, ease-in-out timing function */
}

.square:hover {
  transform: rotate(45deg);
}
```

## Shorthand Notation

In shorthand notation, the order of the properties matters only for time-related properties (duration and delay). The order of the other properties does not matter:

```css
.square {
  width: 200px;
  height: 200px;
  background-color: purple;
  transition: ease-in-out 1s 0.5s transform; /* order of each property does not matter */
                                             /* ONLY time related properties have order -> duration, delay */
}
```

## Multiple Properties

We can also specify multiple properties to transition:

```css
.square {
  width: 200px;
  height: 200px;
  background-color: purple;
  transition-property: transform, background-color; /* Specify multiple properties to transition */
  transition-duration: 1s; /* Only one duration for all properties */
}

.square:hover {
  transform: rotate(45deg);
  background-color: orange; /* Change background color on hover */
}
```

We can also specify different durations for each property:

```css
.square {
  width: 200px;
  height: 200px;
  background-color: purple;
  transition-property: transform, background-color;
  transition-duration: 1s, 0.5s; /* Different durations for each property */
}

.square:hover {
  transform: rotate(45deg); /* 1s duration */
  background-color: orange; /* 0.5s duration */
}
```

We can shorten the above code into a single `transition` property:

```css
.square {
  width: 200px;
  height: 200px;
  background-color: purple;
  transition: transform 1s, background-color 0.5s; /* Shorthand for multiple properties with different durations */
}

.square:hover {
  transform: rotate(45deg);
  background-color: orange;
}
```

## Transitioning all properties

We can also use the keyword `all` to transition all properties but it can not define different durations for each property:

```css
.square {
  width: 200px;
  height: 200px;
  background-color: purple;
  transition: all 1s; /* Transition all properties with the same duration */
}

.square:hover {
  transform: rotate(45deg);
  background-color: orange;
}
```


