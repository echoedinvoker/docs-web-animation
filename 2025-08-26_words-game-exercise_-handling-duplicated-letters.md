# words game exercise: handling duplicated letters

```js
//...

document.addEventListener("DOMContentLoaded", () => {
  // ...

  function initDOM() {
    // ...

    letters.forEach((letter) => {
      const letterWrapper = document.createElement("div");
      letterWrapper.classList.add(`letter`, `letter-${letter}`);
      letterWrapper.style.viewTransitionName = `letter-${letter}`; // set unique view transition name for each letter

      const button = document.createElement("button");
      button.innerText = letter.toUpperCase();
      button.addEventListener("click", () => {
        chooseLetter(letter);
      });

      letterWrapper.append(button);

      lettersEl.appendChild(letterWrapper);
    });

    // ...
  }

  // ...

  function populateGuess(letter, indexes) {
    const slots = document.querySelector(".guess.main").children;
    indexes.forEach((index, _index) => {
      const slotItem = document.createElement("div");
      slotItem.innerText = letter.toUpperCase();
      slotItem.classList.add("letter", `letter-${letter}`);
      slotItem.style.viewTransitionName = `letter-${letter}`;  // duplicate view transition name, error occurs here
      slots[index].appendChild(slotItem);
    });
  }

  async function newGame() {
    try {
      // Get a new word from the API
      // const res = await fetch(
      //   `https://random-word-api.herokuapp.com/word?length=${WORD_LENGTH}&number=1`
      // ).then((r) => r.json());
      // console.log(res[0]);
      // Clear any answer, lives, points, guess and chosenLetters from previous games
      // answer = res[0].split("");
      answer = "spoon".split("");
      //       ^^^^^^^ instead of random word, here we hardcode a word with duplicated letters for testing
      lives = MAX_LIVES;
      points = 0;
      guess = Array(WORD_LENGTH).fill(""); // ["","","","",""]
      chosenLetters = [];

      // Re-initialize the DOM to clear any old state
      initDOM();
    } catch (e) {
      console.log(e);
    }
  }

  newGame();

  // ...
});
```

In the example above, when there are duplicate letters in the answer, errors may occur due to duplicate view transition names in the guess slot, causing the animation to not appear.


## Simple solution, one movement transition animation, others fade in transition animation

Below is a simple solution, we make one of the guess slots have an animation from the keyboard letter, while the other guess slot with repeated letters appears in a fade-in manner.

```
Unexpected duplicate view-transition-name: letter-o 
    <div class="letter letter-o" style="view-transition-name: letter-o;">O</div>
    <div class="letter letter-o" style="view-transition-name: letter-o;">O</div>
```


```js
  function populateGuess(letter, indexes) {
    const slots = document.querySelector(".guess.main").children;
    indexes.forEach((pos, index) => {
      const slotItem = document.createElement("div");
      slotItem.innerText = letter.toUpperCase();
      slotItem.classList.add("letter", `letter-${letter}`);
      slotItem.style.viewTransitionName = `letter-${letter}-${index}`;
      //                                                   ^^^^^^^^^ add index to make it unique
      slots[pos].appendChild(slotItem);
    });
  }
```


```js
  function initDOM() {
    // ...

    letters.forEach((letter) => {
      const letterWrapper = document.createElement("div");
      letterWrapper.classList.add(`letter`, `letter-${letter}`);
      letterWrapper.style.viewTransitionName = `letter-${letter}-0`;
      //                                                        ^^ add -0 to make it at least match the first one view transition name of the guess slots

      // ...
    });

    // ...
  }
```


## Let all duplicated letters has movement transition animation

In order to have transition animations from the keyboard letter to the guess slot for repeated letters, we need to calculate the number of times each letter appears in the answer, and based on that, add some cloned buttons to display the animation.

```js
// ...

document.addEventListener("DOMContentLoaded", () => {
  // ...

  const letters = [..."abcdefghijklmnopqrstuvwxyz"];

  // ...

  let answer;

  // ...

  function initDOM() {

    // ...

    letters.forEach((letter) => {

      // count each letter occurences in the answer
      let occurences = 0;
      answer.forEach(l => {
        if (l === letter) occurences += 1;
      })

      const letterWrapper = document.createElement("div");
      letterWrapper.classList.add(`letter`, `letter-${letter}`);
      // letterWrapper.style.viewTransitionName = `letter-${letter}-0`;

      const button = document.createElement("button");
      button.innerText = letter.toUpperCase();

      // set view transition name to button instead of wrapper
      button.style.viewTransitionName = `letter-${letter}-0`;

      button.addEventListener("click", () => {
        chooseLetter(letter);
      });

      letterWrapper.append(button);

      lettersEl.appendChild(letterWrapper);
    });

    // ...
  }

  // ...

});
```


We may have multiple identical letter buttons, so we create an array to store each letter.


```js
    letters.forEach((letter) => {

      let occurences = 0;

      answer.forEach(l => {
        if (l === letter) occurences += 1;
      })

      const letterWrapper = document.createElement("div");
      letterWrapper.classList.add(`letter`, `letter-${letter}`);

      const buttons = []; // add array of buttons

      const button = document.createElement("button");
      button.innerText = letter.toUpperCase();
      button.style.viewTransitionName = `letter-${letter}-0`;
      button.addEventListener("click", () => {
        chooseLetter(letter);
      });

      buttons.push(button) // join button to array

      letterWrapper.append(...buttons);
      //                   ^^^^^^^^^^ append a list of buttons instead of only one

      lettersEl.appendChild(letterWrapper);
    });
```

Based on the repetition of letters in the answer, we add cloned buttons to the buttons array and set the corresponding view transition names.


```js
    letters.forEach((letter) => {

      let occurences = 0;

      answer.forEach(l => {
        if (l === letter) occurences += 1;
      })

      const letterWrapper = document.createElement("div");
      letterWrapper.classList.add(`letter`, `letter-${letter}`);

      const buttons = [];

      const button = document.createElement("button");
      button.innerText = letter.toUpperCase();
      button.style.viewTransitionName = `letter-${letter}-0`;
      button.addEventListener("click", () => {
        chooseLetter(letter);
      });

      buttons.push(button)

      // only clone button when letter occurences greater than one time
      if (occurences > 1) {
        Array(occurences - 1).fill(null).forEach((n, i) => {
          const buttonClone = button.cloneNode(true);
          //                                   ^^^^ for deep cloning
          buttonClone.style.viewTransitionName = `letter-${letter}-${i+1}`;
          //                                                       ^^^^^^ to match the guess slot view names
          buttonClone.classList.add('clone'); // for styling the cloned button
          buttons.push(buttonClone);
        })
      }

      letterWrapper.append(...buttons);

      lettersEl.appendChild(letterWrapper);
    });
```


Now, because multiple cloned buttons have been added, multiple duplicate English letters will also be displayed on the keyboard. We must use CSS to make the duplicate letters overlap and appear as one.


```css
#letters button {
  background-color: var(--beige);
  border: 1px solid #ffdca9;
  display: inline-flex;
  width: 40px;
  height: 40px;
  align-items: center;
  justify-content: center;
  padding: 0;
  margin: 10px;
  border-radius: 3px;
  box-shadow: 0 0 5px 5px rgba(1, 1, 1, 0.2);
  cursor: pointer;
  transition: 0.3s;
  position: relative;
  z-index: 10; /* set z-index greater than cloned button in order to overlap them */
}
#letters .letter {
  position: relative;
}

/* let cloned button position absolutely behind the origin button */
#letters .letter button.clone {
  position: absolute;
  left: 0;
  top: 0;
  z-index: 9;
}
```

Now on the keyboard, each English word should only have one, and when there are multiple letters in the answer, the transition animation can be displayed correctly.
