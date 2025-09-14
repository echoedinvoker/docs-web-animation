# words game exercise: modal transition animation

```js
// ...

document.addEventListener("DOMContentLoaded", () => {
  // ...
  const modal = document.querySelector(".result");
  const modalTitle = document.querySelector(".result .title");
  const coinsTextEl = document.querySelector(
    ".header .info .coins .coins-count"
  );

  // ...

  function updateDOM(letter, indexes, result) {
    // if we have a final result, display the modal
    if (result) {
      displayModal(result);
    }
    // ...
  }

  function initDOM() {
    // ...

    // Hide Modal if it was shown
    modal.classList.add("hide");

    // ...
  }

  // Display the modal and populate the title based on the result.
  function displayModal(result) {
    modalTitle.innerText = result === "win" ? "Good Job!" : "Game Over!";
    modal.classList.remove("hide");
  }

  // ...
});
```


```css
::view-transition-old(*):only-child {
  animation: pop-out 0.25s ease-in;
}
::view-transition-new(*):only-child {
  animation: pop-in 0.25s ease-out;
}
```

## Try to add transition animations when modal appear and disapper

In the code above, we can see that the appearance of the modal is controlled by adding or removing the class `modal` using JS. Therefore, we only need to add or remove the view transition name at the same location.


```js
```js
// ...

document.addEventListener("DOMContentLoaded", () => {

  // ...

  function initDOM() {
    // ...

    modal.classList.add("hide");
    modal.style.viewTransitionName = 'none'; // remove view transition name from modal

    // ...
  }

  function displayModal(result) {
    modalTitle.innerText = result === "win" ? "Good Job!" : "Game Over!";
    modal.classList.remove("hide");
    modal.style.viewTransitionName = 'modal'; // add view transition name to modal
  }

  // ...
});
```


```
::view-transition-group(modal)
    ::view-transition-image-pair(modal)
        ::view-transition-new(modal)  <-- only new, no old
```


## Fix the issue of no pop-out transition animation

We are trying to identify the issue where the modal does not have a pop-out transition animation when opening a new game, but instead disappears directly. We found that in the newGame function, initDOM() is used to update the DOM to its initial state, but it is not wrapped in `document.startViewTransition()`, which is the problem.


```js
  async function newGame() {
    try {
      answer = "spoon".split("");
      lives = MAX_LIVES;
      points = 0;
      guess = Array(WORD_LENGTH).fill(""); // ["","","","",""]
      chosenLetters = [];

      // Re-initialize the DOM to clear any old state
      initDOM(); // <-- update DOM but not wrap by `document.startViewTransition()`
    } catch (e) {
      console.log(e);
    }
  }
```


```js
      document.startViewTransition(() => { // simply wrap it with the `document.startViewTransition()`
        initDOM();
      })
```


## Specified transition animations for modal

In the previous section, we used a global transition animation on the modal. We can write a specific transition animation for the modal at the bottom of the CSS file as follows:

```css
@keyframes scale-in {
  from {
    opacity: 0;
    transform: scale(2);
  }
  to {
    opacity: 1;
    transform: scale(1);
  }
}

@keyframes scale-out {
  from {
    opacity: 1;
    transform: scale(1);
  }
  to {
    opacity: 0;
    transform: scale(2);
  }
}
::view-transition-old(modal):only-child {
/*                    ^^^^^ on top of wildcard(*) */
  animation: 0.25s ease-in scale-out;
  /*                       ^^^^^^^^^ intead of `pop-out`, using `scale-out` */
}

::view-transition-new(modal):only-child {
  animation: 0.25s ease-out scale-in;
}
```

Now our modal has better transition animations.
